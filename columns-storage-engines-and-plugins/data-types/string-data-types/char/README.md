# CHAR

This article covers the CHAR data type. See [CHAR Function](/built-in-functions/string-functions/char-function/) for the function.

## Syntax

```sql
[NATIONAL] CHAR[(M)] [CHARACTER SET charset_name] [COLLATE collation_name]
```

## Description

A fixed-length string that is always right-padded with spaces to the specified
length when stored. `M` represents the column length in characters. The range
of `M` is `0` to `255`. If `M` is omitted, the length is `1`.

CHAR(0) columns can contain 2 values: an empty string or NULL. Such columns cannot be part of an index. The [CONNECT](/columns-storage-engines-and-plugins/storage-engines/connect/) storage engine does not support CHAR(0).

<strong>Note:</strong> Trailing spaces are removed when `CHAR` values are retrieved
unless the `PAD_CHAR_TO_FULL_LENGTH` [SQL mode](/kb/en/sql_mode/) is enabled.

Before [MariaDB 10.2](/kb/en/what-is-mariadb-102/), all collations were of type PADSPACE, meaning that CHAR (as well as [VARCHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/varchar/) and [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text/)) values are compared without regard for trailing spaces. This does not apply to the [LIKE](/built-in-functions/string-functions/like/) pattern-matching operator, which takes into account trailing spaces.

If a unique index consists of a column where trailing pad characters are stripped or ignored, inserts into that column where values differ only by the number of trailing pad characters will result in a duplicate-key error.

## Examples

Trailing spaces:

```sql
CREATE TABLE strtest (c CHAR(10));
INSERT INTO strtest VALUES('Maria   ');

SELECT c='Maria',c='Maria   ' FROM strtest;
+-----------+--------------+
| c='Maria' | c='Maria   ' |
+-----------+--------------+
|         1 |            1 |
+-----------+--------------+

SELECT c LIKE 'Maria',c LIKE 'Maria   ' FROM strtest;
+----------------+-------------------+
| c LIKE 'Maria' | c LIKE 'Maria   ' |
+----------------+-------------------+
|              1 |                 0 |
+----------------+-------------------+
```

## NO PAD Collations

##### MariaDB starting with [10.2](/kb/en/what-is-mariadb-102/)

NO PAD collations regard trailing spaces as normal characters. You can get a list of all NO PAD collations by querying the [Information Schema Collations table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-collations-table/), for example:

```sql
SELECT collation_name FROM information_schema.collations 
  WHERE collation_name LIKE "%nopad%";  
+------------------------------+
| collation_name               |
+------------------------------+
| big5_chinese_nopad_ci        |
| big5_nopad_bin               |
...
```

## See Also

- [CHAR Function](/built-in-functions/string-functions/char-function/)
- [VARCHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/varchar/)
- [BINARY](/columns-storage-engines-and-plugins/data-types/string-data-types/binary/)
- [Data Type Storage Requirements](/columns-storage-engines-and-plugins/data-types/data-type-storage-requirements/)