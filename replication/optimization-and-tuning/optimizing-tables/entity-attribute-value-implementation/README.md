# Entity-Attribute-Value Implementation

## The desires

- Open-ended set of "attributes" (key=value) for each "entity". That is, the list of attributes is not known at development time, and will grow in the future. (This makes one column per attribute impractical.)
- "ad hoc" queries testing attributes.
- Attribute values come in different types (numbers, strings, dates, etc.)
- Scale to lots of entities, yet perform well.

It goes by various names

- EAV -- Entity - Attribute - Value
- key-value
- RDF -- This is a flavor of EAV
- MariaDB has dynamic columns that look something like the solution below, with the added advantage of being able to index the columns otherwise hidden in the blob. (There are caveats.)
- MySQL 5.7 Has JSON datatype, plus functions to access parts
- MongoDB, CouchDB -- and others -- Not SQL-based.

## Bad solution

- Table with 3 columns: entity_id, key, value
- The "value" is a string, or maybe multiple columns depending on datatype or other kludges.
- a JOIN b ON a.entity=b.entity AND b.key='x' JOIN c ON ... WHERE a.value=... AND b.value=...

## The problems

- The SELECTs get messy -- multiple JOINs
- Datatype issues -- It's clumsy to be putting numbers into strings
- Numbers stored in [VARCHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/varchar/) do not compare 'correctly', especially for range tests.
- Bulky.
- Dedupping the values is clumsy.

## A solution

Decide which columns need to be searched/sorted by SQL queries. No, you don't need all the columns to be searchable or sortable. Certain columns are frequently used for selection; identify these. You probably won't use all of them in all queries, but you will use some of them in every query.

The solution uses one table for all the EAV stuff. The columns include the searchable fields plus one [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob/). Searchable fields are declared appropriately ([INT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/int/), [TIMESTAMP](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/timestamp/), etc). The BLOB contains JSON-encoding of all the extra fields.

The table should be [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/), hence it should have a PRIMARY KEY. The entitity_id is the 'natural' PK. Add a small number of other indexes (often 'composite') on the searchable fields. [PARTITIONing](/kb/en/managing-mariadb-partitioning/) is unlikely to be of any use, unless the Entities should purged after some time. (Example: News Articles)

## But what about the ad hoc queries?

You have included the most important fields to search on -- date, category, etc. These should filter the data down significantly. When you also need to filter on something more obscure, that will be handled differently. The application code will look at the BLOB for that; more on this later.

## Why it works

- You are not really going to search on more than a few fields.
- The disk footprint is smaller; Smaller --&gt; More cacheable --&gt; Faster
- It needs no JOINs
- The indexes are useful
- The one table has one row per entity, and can grow as needed. (EAV needs many rows per entity.)
- Performance is as good as the indexes you have on the 'searchable fields'.
- Optionally, you can duplicate the indexed fields in the BLOB.
- Values missing from 'searchable fields' would need to be NULL (or whatever), and the code would need to deal with such.

## Details on the BLOB/JSON

- Build the extra (or all) key-value pairs in a hash (associative array) in your application. Encode it. COMPRESS it. Insert that string into the [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob/).
- JSON is recommended, but not mandatory; it is simpler than XML. Other serializations (eg, YAML) could be used.
- COMPRESS the JSON and put it into a [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob/) (or [MEDIUMBLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/mediumblob/)) instead of a [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text/) field. Compression gives about 3x shrinkage.
- When SELECTing, UNCOMPRESS the blob. Decode the string into a hash. You are now ready to interrogate/display any of the extra fields.
- If you choose to use the JSON features of MariaDB or 5.7, you will have to forgo the compression feature described.
- MySQL 5.7.8's JSON native JSON datatype uses a binary format for more efficient access.

## Conclusions

- Schema is reasonably compact (compression, real datatypes, less redundancy, etc, than EAV)
- Queries are fast (since you have picked 'good' indexes)
- Expandable (JSON is happy to have new fields)
- Compatible (No 3rd party products, just supported products)
- Range tests work (unlike storing [INTs](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/int/) in [VARCHARs](/columns-storage-engines-and-plugins/data-types/string-data-types/varchar/))
- (Drawback) Cannot use the non-indexed attributes in WHERE or ORDER BY clauses, must deal with that in the app. (MySQL 5.7 partially alleviates this.)

## Postlog

Posted Jan, 2014; Refreshed Feb, 2016.

- MariaDB's [Dynamic Columns](/sql-statements-structure/nosql/dynamic-columns/)
- [MySQL 5.7's JSON](https://dev.mysql.com/doc/refman/5.7/en/json.html)

This looks very promising; I will need to do more research to see how much of this article is obviated by it: [Using MySQL as a Document Store in 5.7](http://dev.mysql.com/doc/refman/5.7/en/document-store.html), 
[more DocStore discussion](http://mysqlserverteam.com/mysql-5-7-12-part-6-mysql-document-store-a-new-chapter-in-the-mysql-story/)

If you insist on EAV, set [optimizer_search_depth=1](/kb/en/server-system-variables/#optimizer_search_depth).

## See also

Rick James graciously allowed us to use this article in the Knowledge Base.

[Rick James' site](http://mysql.rjweb.org/) has other useful tips, how-tos,
optimizations, and debugging tips.

Original source: [http://mysql.rjweb.org/doc.php/eav](http://mysql.rjweb.org/doc.php/eav)