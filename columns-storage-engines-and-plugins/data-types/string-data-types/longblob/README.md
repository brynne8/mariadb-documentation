# LONGBLOB

## Syntax

```sql
LONGBLOB
```

## Description

A [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob/) column with a 
maximum length of 4,294,967,295 bytes or 4GB (2<sup>32</sup> - 1). The effective maximum length of LONGBLOB columns depends on the
configured maximum packet size in the client/server protocol and
available memory. Each LONGBLOB value is stored using a four-byte
length prefix that indicates the number of bytes in the value.

### Oracle Mode

##### MariaDB starting with [10.3](/kb/en/what-is-mariadb-103/)

In [Oracle mode from MariaDB 10.3](/kb/en/sql_modeoracle-from-mariadb-103/#synonyms-for-basic-sql-types), `BLOB` is a synonym for `LONGBLOB`.

## See Also

- [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob/)
- [BLOB and TEXT Data Types](/columns-storage-engines-and-plugins/data-types/string-data-types/blob-and-text-data-types/)
- [Data Type Storage Requirements](/columns-storage-engines-and-plugins/data-types/data-type-storage-requirements/)
- [Oracle mode from MariaDB 10.3](/kb/en/sql_modeoracle-from-mariadb-103/#synonyms-for-basic-sql-types)