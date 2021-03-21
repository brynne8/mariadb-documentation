# VARCHAR

## Syntax

```sql
[NATIONAL] VARCHAR(M) [CHARACTER SET charset_name] [COLLATE collation_name]
```

## Description

A variable-length string. M represents the maximum column length in
characters. The range of M is 0 to 65,532. The effective maximum
length of a VARCHAR is subject to the maximum row size and the character set used. For
example, utf8 characters can require up to three bytes per character,
so a VARCHAR column that uses the utf8 character set can be declared
to be a maximum of 21,844 characters.

<strong>Note:</strong> For the [ColumnStore](/kb/en/columnstore/) engine, M represents the maximum column length in
bytes.

MariaDB stores VARCHAR values as a one-byte or two-byte length prefix
plus data. The length prefix indicates the number of bytes in the
value. A VARCHAR column uses one length byte if values require no more
than 255 bytes, two length bytes if values may require more than 255
bytes.

MariaDB follows the standard SQL specification, and does not remove trailing spaces from VARCHAR values.

VARCHAR(0) columns can contain 2 values: an empty string or NULL. Such columns cannot be part of an index. The [CONNECT](/columns-storage-engines-and-plugins/storage-engines/connect/) storage engine does not support VARCHAR(0).

VARCHAR is shorthand for CHARACTER VARYING. NATIONAL VARCHAR is the
standard SQL way to define that a VARCHAR column should use some
predefined character set. MariaDB uses utf8 as this
predefined character set, as does MySQL 4.1 and up.
NVARCHAR is shorthand for NATIONAL VARCHAR.

Before [MariaDB 10.2](/kb/en/what-is-mariadb-102/), all MariaDB [collations](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/) were of type `PADSPACE`, meaning that VARCHAR (as well as [CHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/char/) and [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text/) values) are compared without regard for trailing spaces. This does not apply to the [LIKE](/built-in-functions/string-functions/like/) pattern-matching operator, which takes into account trailing spaces. From [MariaDB 10.2](/kb/en/what-is-mariadb-102/), a number of [NO PAD collations](/kb/en/supported-character-sets-and-collations/#no-pad-collations) are available.

If a unique index consists of a column where trailing pad characters are stripped or ignored, inserts into that column where values differ only by the number of trailing pad characters will result in a duplicate-key error.

## Examples

The following are equivalent:

```sql
VARCHAR(30) CHARACTER SET utf8
NATIONAL VARCHAR(30)
NVARCHAR(30)
NCHAR VARCHAR(30)
NATIONAL CHARACTER VARYING(30)
NATIONAL CHAR VARYING(30)
```

Trailing spaces:

```sql
CREATE TABLE strtest (v VARCHAR(10));
INSERT INTO strtest VALUES('Maria   ');

SELECT v='Maria',v='Maria   ' FROM strtest;
+-----------+--------------+
| v='Maria' | v='Maria   ' |
+-----------+--------------+
|         1 |            1 |
+-----------+--------------+

SELECT v LIKE 'Maria',v LIKE 'Maria   ' FROM strtest;
+----------------+-------------------+
| v LIKE 'Maria' | v LIKE 'Maria   ' |
+----------------+-------------------+
|              0 |                 1 |
+----------------+-------------------+
```

## Truncation

- Depending on whether or not [strict sql mode](/kb/en/sql-mode/#strict-mode) is set, you will either get a warning or an error if you try to insert a string that is too long into a VARCHAR column. If the extra characters are spaces, the spaces that can't fit will be removed and you will always get a warning, regardless of the [sql mode](/kb/en/sql_mode/) setting.

## Difference Between VARCHAR and [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text/)

- VARCHAR columns can be fully indexed. [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text/) columns can only be indexed over a specified length.
- Using [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text/) or [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob/) in a [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/) query that uses temporary tables for storing intermediate results will force the temporary table to be disk based (using the [Aria storage engine](/columns-storage-engines-and-plugins/storage-engines/aria/aria-storage-engine/) instead of the [memory storage engine](/replication/optimization-and-tuning/query-optimizations/guiduuid-performance/mariadb/memory-storage-engine/), which is a bit slower. This is not that bad as the [Aria storage engine](/columns-storage-engines-and-plugins/storage-engines/aria/aria-storage-engine/) caches the rows in memory. To get the benefit of this, one should ensure that the [aria_pagecache_buffer_size](/kb/en/aria-system-variables/#aria_pagecache_buffer_size) variable is big enough to hold most of the row and index data for temporary tables.

## Oracle Mode

##### MariaDB starting with [10.3](/kb/en/what-is-mariadb-103/)

In [Oracle mode from MariaDB 10.3](/kb/en/sql_modeoracle-from-mariadb-103/#synonyms-for-basic-sql-types), `VARCHAR2` is a synonym.

### For Storage Engine Developers

- Internally the full length of the VARCHAR column is allocated inside each TABLE objects record[] structure. As there are three such buffers, each open table will allocate 3 times max-length-to-store-varchar bytes of memory.
- [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text/) and [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob/) columns are stored with a pointer (4 or 8 bytes) + a 1-4 bytes length.  The [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text/) data is only stored once. This means that internally `TEXT` uses less memory for each open table but instead has the additional overhead that each `TEXT` object needs to be allocated and freed for each row access (with some caching in between).

## See Also

- [VARBINARY](/columns-storage-engines-and-plugins/data-types/string-data-types/varbinary/)
- [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text/)
- [CHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/char/)
- [Character Sets and Collations](/kb/en/data-types-character-sets-and-collations/)
- [Data Type Storage Requirements](/columns-storage-engines-and-plugins/data-types/data-type-storage-requirements/)
- [Oracle mode from MariaDB 10.3](/kb/en/sql_modeoracle-from-mariadb-103/#synonyms-for-basic-sql-types)