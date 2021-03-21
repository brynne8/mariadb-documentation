# Data Sampling: Techniques for Efficiently Finding a Random Row

## Fetching random rows from a table (beyond ORDER BY RAND())

### The problem

One would like to do "SELECT ... ORDER BY RAND() LIMIT 10" to get 10 rows at random. But this is slow. The optimizer does

- Fetch all the rows -- this is costly
- Append [RAND()](/built-in-functions/numeric-functions/rand) to the rows
- Sort the rows -- also costly
- Pick the first 10.

All the algorithms given below are "fast", but most introduce flaws:

- Bias -- some rows are more like to be fetched than others.
- Repetitions -- If two random sets contain the same row, they are likely to contain other dups.
- Sometimes failing to fetch the desired number of rows.

"Fast" means avoiding reading all the rows. There are many techniques that require a full table scan, or at least an index scan. They are not acceptable for this list. There is even a technique that averages half a scan; it is relegated to a footnote.

### Metrics

Here's a way to measure performance without having a big table.

```sql
    FLUSH STATUS;
    SELECT ...;
    SHOW SESSION STATUS LIKE 'Handler%';
```

If some of the "Handler" numbers look like the number of rows in the table, then there was a table scan.

None of the queries presented here need a full table (or index) scan. Each has a time proportional to the number of rows returned.

Virtually all published algorithms involve a table scan. The previously published version of this blog had, embarrassingly, several algorithms that had table scans.

Sometimes the scan can be avoided via a subquery. For example, the first of these will do a table scan; the second will not.

```sql
SELECT *  FROM RandTest AS a
  WHERE id = FLOOR(@min + (@max - @min + 1) * RAND());  -- BAD: table scan
SELECT *
 FROM RandTest AS a
 JOIN (
   SELECT FLOOR(@min + (@max - @min + 1) * RAND()) AS id -- Good; single eval.
      ) b  USING (id);
```

### Case: Consecutive AUTO_INCREMENT without gaps, 1 row returned

- Requirement: [AUTO_INCREMENT](/columns-storage-engines-and-plugins/data-types/auto_increment) id
- Requirement: No gaps in id

```sql
  SELECT r.*
      FROM (
          SELECT FLOOR(mm.min_id + (mm.max_id - mm.min_id + 1) * RAND()) AS id
              FROM (
                  SELECT MIN(id) AS min_id,
                         MAX(id) AS max_id
                      FROM RandTest
                   ) AS mm
           ) AS init
      JOIN  RandTest AS r  ON r.id = init.id;
```

(Of course, you might be able to simplify this. For example, min_id is likely to be 1. Or precalculate limits into @min and @max.)

### Case: Consecutive AUTO_INCREMENT without gaps, 10 rows

- Requirement: AUTO_INCREMENT id
- Requirement: No gaps in id
- Flaw: Sometimes delivers fewer than 10 rows

```sql
  -- First select is one-time:
  SELECT @min := MIN(id),
         @max := MAX(id)
      FROM RandTest;
  SELECT DISTINCT *
      FROM RandTest AS a
      JOIN (
          SELECT FLOOR(@min + (@max - @min + 1) * RAND()) AS id
              FROM RandTest
              LIMIT 11    -- more than 10 (to compensate for dups)
           ) b  USING (id)
      LIMIT 10;           -- the desired number of rows
```

The FLOOR expression could lead to duplicates, hence the inflated inner LIMIT. There could (rarely) be so many duplicates that the inflated LIMIT leads to fewer than the desired 10 different rows. One approach to that Flaw is to rerun the query if it delivers too few rows.

A variant:

```sql
  SELECT r.*
      FROM (
          SELECT FLOOR(mm.min_id + (mm.max_id - mm.min_id + 1) * RAND()) AS id
              FROM (
                  SELECT MIN(id) AS min_id,
                         MAX(id) AS max_id
                      FROM RandTest
                   ) AS mm
              JOIN ( SELECT id dummy FROM RandTest LIMIT 11 ) z
           ) AS init
      JOIN  RandTest AS r  ON r.id = init.id
      LIMIT 10;
```

Again, ugly but fast, regardless of table size.

### Case: AUTO_INCREMENT with gaps, 1 or more rows returned

- Requirement: AUTO_INCREMENT, possibly with gaps due to DELETEs, etc
- Flaw: Only semi-random (rows do not have an equal chance of being picked), but it does partially compensate for the gaps
- Flaw: The first and last few rows of the table are less likely to be delivered.

This gets 50 "consecutive" ids (possibly with gaps), then delivers a random 10 of them.

```sql
  -- First select is one-time:
  SELECT @min := MIN(id),
         @max := MAX(id)
      FROM RandTest;
  SELECT a.*
      FROM RandTest a
      JOIN ( SELECT id FROM
              ( SELECT id
                  FROM ( SELECT @min + (@max - @min + 1 - 50) * RAND() 
                    AS start FROM DUAL ) AS init
                  JOIN RandTest y
                  WHERE    y.id > init.start
                  ORDER BY y.id
                  LIMIT 50           -- Inflated to deal with gaps
              ) z ORDER BY RAND()
             LIMIT 10                -- number of rows desired (change to 1 if looking for a single row)
           ) r ON a.id = r.id;
```

Yes, it is complex, but yes, it is fast, regardless of the table size.

### Case: Extra FLOAT column for randomizing

(Unfinished: need to check these.)

Assuming `rnd` is a FLOAT (or DOUBLE) populated with RAND() and INDEXed:

- Requirement: extra, indexed, FLOAT column
- Flaw: Fetches 10 adjacent rows (according to `rnd`), hence not good randomness
- Flaw: Near 'end' of table, can't find 10 rows.

```sql
  SELECT r.*
      FROM ( SELECT RAND() AS start FROM DUAL ) init
      JOIN RandTest r
      WHERE r.rnd >= init.start
      ORDER BY r.rnd
      LIMIT 10;
```

- These two variants attempt to resolve the end-of-table flaw:

```sql
  SELECT r.*
      FROM ( SELECT RAND() * ( SELECT rnd
                        FROM RandTest
                        ORDER BY rnd DESC
                        LIMIT 10,1 ) AS start
           ) AS init
      JOIN RandTest r
      WHERE r.rnd > init.start
      ORDER BY r.rnd
      LIMIT 10;


  SELECT @start := RAND(),
         @cutoff := CAST(1.1 * 10 + 5 AS DECIMAL(20,8)) / TABLE_ROWS
      FROM information_schema.TABLES
      WHERE TABLE_SCHEMA = 'dbname'
        AND TABLE_NAME = 'RandTest'; -- 0.0030
  SELECT d.*
      FROM (
          SELECT a.id
              FROM RandTest a
              WHERE rnd BETWEEN @start AND @start + @cutoff
           ) sample
      JOIN RandTest d USING (id)
      ORDER BY rand()
      LIMIT 10;
```

### Case: UUID or MD5 column

- Requirement: UUID/GUID/MD5/SHA1 column exists and is indexed.
- Similar code/benefits/flaws to AUTO_INCREMENT with gaps.
- Needs 7 random HEX digits:

```sql
RIGHT( HEX( (1<<24) * (1+RAND()) ), 6)
```

can be used as a `start` for adapting a gapped AUTO_INCREMENT case. If the field is BINARY instead of hex, then

```sql
UNHEX(RIGHT( HEX( (1<<24) * (1+RAND()) ), 6))
```

## See also

Rick James graciously allowed us to use this article in the Knowledge Base.

[Rick James' site](http://mysql.rjweb.org/) has other useful tips, how-tos,
optimizations, and debugging tips.

Original source: [http://mysql.rjweb.org/doc.php/random](http://mysql.rjweb.org/doc.php/random)