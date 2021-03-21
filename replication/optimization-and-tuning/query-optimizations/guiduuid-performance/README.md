# GUID/UUID Performance

## The problem

GUIDs/UUIDs (Globally/Universal Unique Identifiers) are very random. Therefore, INSERTing into an index means jumping around a lot. Once the index is too big to be cached, most INSERTs involve a disk hit. Even on a beefy system, this limits you to a few hundred INSERTs per second.

[MariaDB's UUID function](/built-in-functions/secondary-functions/miscellaneous-functions/uuid).

This blog is mostly eliminated in MySQL 8.0 with the advent of the following function:
[UUID_TO_BIN(str, swap_flag)](https://dev.mysql.com/doc/refman/8.0/en/miscellaneous-functions.html#function_uuid-to-bin).

## Why it is a problem

A 'standard' GUID/UUID is composed of the time, machine identification and some other stuff. The combination should be unique, even without coordination between different computers that could be generating UUIDs simultaneously.

The top part of the GUID/UUID is the bottom part of the current time. The top part is the primary part of what would be used for placing the value in an ordered list (INDEX). This cycles in about 7.16 minutes.

Some math... If the index is small enough to be cached in RAM, each insert into the index is CPU only, with the writes being delayed and batched. If the index is 20 times as big as can be cached, then 19 out of 20 inserts will be a cache miss. (This math applies to any "random" index.)

## Second problem

36 characters is bulky. If you are using that as a PRIMARY KEY in InnoDB and you have secondary keys, remember that each secondary key has an implicit copy of the PK, thereby making it bulky.

It is tempting to declare the UUID [VARCHAR(36)](/columns-storage-engines-and-plugins/data-types/string-data-types/varchar). And, since you probably are thinking globally, so you have [CHARACTER SET](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets) utf8 (or utf8mb4). For utf8:

- 2 - Overhead for VAR
- 36 - chars
- 3 (or 4) bytes per character for utf8 (or utf8mb4)
So, max length = 2+3*36 = 110 (or 146) bytes. For temp tables 108 (or 144) is actually used if a [MEMORY](mariadb/memory-storage-engine) table is used.

To compress

- utf8 is unnecessary (ascii would do); but this is obviated by the next two steps
- Toss dashes
- [UNHEX](/built-in-functions/string-functions/unhex)
Now it will fit in 16 bytes: [BINARY(16)](/columns-storage-engines-and-plugins/data-types/string-data-types/binary)

## Combining the problems and crafting a solution

But first, a caveat. This solution only works for ["Time based" / "Version 1" UUIDs](https://en.wikipedia.org/wiki/Universally_unique_identifier)
They are recognizable by the "1" at the beginning of the third clump.

The manual's sample: 6ccd780c-baba-1026-9564-0040f4311e29 . A more current value (after a few years): 49ea2de3-17a2-11e2-8346-001eecac3efa . Notice how the 3rd part has slowly changed over time? Let's data is rearranged, thus:

```sql
      1026-baba-6ccd780c-9564-0040f4311e29
      11e2-17a2-49ea2de3-8346-001eecac3efa
      11e2-17ac-106762a5-8346-001eecac3efa -- after a few more minutes
```

Now we have a number that increases nicely over time. Multiple sources won't be quite in time order, but they will be close. The "hot" spot for inserting into an INDEX(uuid) will be rather narrow, thereby making it quite cacheable and efficient.

If your SELECTs tend to be for "recent" uuids, then they, too, will be easily cached. If, on the other hand, your SELECTs often reach for old uuids, they will be random and not well cached. Still, improving the INSERTs will help the system overall.

## Code to do it

Let's make [Stored Functions](/programming-customizing-mariadb/stored-routines/stored-functions) to do the messy work of the two actions:

- Rearrange fields
- Convert to/from BINARY(16)

```sql
    DELIMITER //

    CREATE FUNCTION UuidToBin(_uuid BINARY(36))
        RETURNS BINARY(16)
        LANGUAGE SQL  DETERMINISTIC  CONTAINS SQL  SQL SECURITY INVOKER
    RETURN
        UNHEX(CONCAT(
            SUBSTR(_uuid, 15, 4),
            SUBSTR(_uuid, 10, 4),
            SUBSTR(_uuid,  1, 8),
            SUBSTR(_uuid, 20, 4),
            SUBSTR(_uuid, 25) ));
    //
    CREATE FUNCTION UuidFromBin(_bin BINARY(16))
        RETURNS BINARY(36)
        LANGUAGE SQL  DETERMINISTIC  CONTAINS SQL  SQL SECURITY INVOKER
    RETURN
        LCASE(CONCAT_WS('-',
            HEX(SUBSTR(_bin,  5, 4)),
            HEX(SUBSTR(_bin,  3, 2)),
            HEX(SUBSTR(_bin,  1, 2)),
            HEX(SUBSTR(_bin,  9, 2)),
            HEX(SUBSTR(_bin, 11))
                 ));

    //
    DELIMITER ;
```

Then you would do things like

```sql
    -- Letting MySQL create the UUID:
    INSERT INTO t (uuid, ...) VALUES (UuidToBin(UUID()), ...);

    -- Creating the UUID elsewhere:
    INSERT INTO t (uuid, ...) VALUES (UuidToBin(?), ...);

    -- Retrieving (point query using uuid):
    SELECT ... FROM t WHERE uuid = UuidToBin(?);

    -- Retrieving (other):
    SELECT UuidFromBin(uuid), ... FROM t ...;
```

Do not flip the WHERE; this will be inefficent because it won't use INDEX(uuid):

```sql
    WHERE UuidFromBin(uuid) = '1026-baba-6ccd780c-9564-0040f4311e29' -- NO
```

## TokuDB

[TokuDB](/columns-storage-engines-and-plugins/storage-engines/tokudb) is a viable engine if you must have UUIDs (even non-type-1) in a huge table. TokuDB is available in MariaDB as a 'standard' engine, making the barrier to entry very low. There are a small number of differences between [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) and TokuDB; I will not go into them here.

Tokudb, with its “fractal” indexing strategy builds the indexes in stages. In contrast, InnoDB inserts index entries “immediately” — actually that indexing is buffered by most of the size of the buffer_pool. To elaborate…

When adding a record to an InnoDB table, here are (roughly) the steps performed to write the data (and PK) and secondary indexes to disk. (I leave out logging, provision for rollback, etc.) First the PRIMARY KEY and data:

- Check for UNIQUEness constraints
- Fetch the BTree block (normally 16KB) that should contain the row (based on the PRIMARY KEY).
- Insert the row (overflow typically occurs 1% of the time; this leads to a block split).
- Leave the page “dirty” in the buffer_pool, hoping that more rows are added before it is bumped out of cache (buffer_pool).. Note that for AUTO_INCREMENT and TIMESTAMP-based PKs, the “last” block in the data will be updated repeatedly before splitting; hence, this delayed write adds greatly to the efficiency. OTOH, a UUID will be very random; when the table is big enough, the block will almost always be flushed before a second insert occurs in that block. &lt;– This is the inefficiency in UUIDs.
Now for any secondary keys:
- All the steps are the same, since an index is essentially a "table" except that the "data" is a copy of the PRIMARY KEY.
- UNIQUEness must be checked immediately — cannot delay the read.
- There are (I think) some other "delays" that avoid some I/O.

Tokudb, on the other hand, does something like

- Write data/index partially sorted records to disk before finding out exactly where it belongs.
- In the background, combine these partially digested blocks. Repeat as needed.
- Eventually move the info into the real table/indexes.

If you are familiar with how sort-merge works, consider the parallels to Tokudb. Each "sort" does some work of ordering things; each "merge" is quite efficient.

To summarize:

- In the extreme (data/index much larger than buffer_pool), InnoDB must read-modify-write one 16KB disk block for each UUID entry.
- Tokudb makes each I/O "count" by merging several UUIDs for each disk block. (Yeah, Toku rereads blocks, but it comes out ahead in the long run.)
- Tokudb excels when the table is really big, which implies high ingestion rate.

## Wrapup

This shows three thing for speeding up usage of GUIDs/UUIDs:

- Shrink footprint (Smaller -&gt; more cacheable -&gt; faster).
- Rearrange uuid to make a "hot spot" to improve cachability.
- Use TokuDB

Note that the benefit of the "hot spot" is only partial:

- Chronologically ordered (or approximately ordered) INSERTs benefit; random ones don't.
- SELECTs/UPDATEs by "recent" uuids benefit; old ones don't benefit.

## Postlog

Thanks to Trey for some of the ideas here.

The tips in this document apply to MySQL, MariaDB, and Percona.

Written Oct, 2012. Added TokuDB, Jan, 2015.

## See also

- [Detailed discussion of UUID indexing](http://stackoverflow.com/questions/28084901/how-does-mysql-determine-if-an-insert-is-unique/28547410#28547410)
- [Graphical display of the random nature of UUID on PRIMARY KEY](http://www.percona.com/blog/2015/04/03/illustrating-primary-key-models-in-innodb-and-their-impact-on-disk-usage/)
- [Benchmarks, etc, by Karthik Appigatla](https://www.percona.com/blog/2014/12/19/store-uuid-optimized-way/)
- [More details on the clock](https://www.famkruithof.net/guid-uuid-timebased.html)
- [Percona benchmarks](https://www.percona.com/blog/2014/12/19/store-uuid-optimized-way/)
- [NHibernate can generate sequential GUIDs](http://nhibernate.info/blog/2009/05/21/using-the-guid-comb-identifier-strategy.html) , but it seems to be backwards.

Rick James graciously allowed us to use this article in the Knowledge Base.

[Rick James' site](http://mysql.rjweb.org/) has other useful tips, how-tos,
optimizations, and debugging tips.

Original source: [http://mysql.rjweb.org/doc.php/uuid](http://mysql.rjweb.org/doc.php/uuid)