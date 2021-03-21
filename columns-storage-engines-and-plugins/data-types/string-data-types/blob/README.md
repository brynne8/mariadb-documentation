# BLOB

## Syntax

```sql
BLOB[(M)]
```

## Description

A `BLOB` column with a maximum length of `65,535` (`2<sup>16</sup> - 1`) bytes. Each
`BLOB` value is stored using a two-byte length prefix that indicates the
number of bytes in the value.

An optional length `M` can be given for this type. If this is done,
MariaDB creates the column as the smallest `BLOB` type large enough to
hold values <em>`M`</em> bytes long.

BLOBS can also be used to store [dynamic columns](/sql-statements-structure/nosql/dynamic-columns/).

Before [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/), `BLOB` and `TEXT` columns could not be assigned a [DEFAULT](/kb/en/create-table/#default) value. This restriction was lifted in [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/).

### Indexing

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/), it is possible to set a Unique index on a column that uses the `BLOB` data type.  In previous releases this was not possible, as the index would only guarantee the uniqueness of a fixed number of characters.

### Oracle Mode

##### MariaDB starting with [10.3](/kb/en/what-is-mariadb-103/)

In [Oracle mode from MariaDB 10.3](/kb/en/sql_modeoracle-from-mariadb-103/#synonyms-for-basic-sql-types), `BLOB` is a synonym for `LONGBLOB`.

## See Also

- [BLOB and TEXT Data Types](/columns-storage-engines-and-plugins/data-types/string-data-types/blob-and-text-data-types/)
- [Data Type Storage Requirements](/columns-storage-engines-and-plugins/data-types/data-type-storage-requirements/)
- [Oracle mode from MariaDB 10.3](/kb/en/sql_modeoracle-from-mariadb-103/#synonyms-for-basic-sql-types)