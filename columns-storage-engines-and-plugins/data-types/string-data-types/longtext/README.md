# LONGTEXT

## Syntax

```sql
LONGTEXT [CHARACTER SET charset_name] [COLLATE collation_name]
```

## Description

A [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text/) column with a maximum length of 4,294,967,295 or 4GB (`2<sup>32</sup> - 1`) characters. The effective maximum length is less if the value contains multi-byte characters. The effective maximum length of LONGTEXT columns also depends on the configured maximum packet size in the client/server protocol and available memory. Each LONGTEXT value is stored using a four-byte length prefix that indicates the number of bytes in the value.

From [MariaDB 10.2.7](/kb/en/mariadb-1027-release-notes/), JSON is an alias for LONGTEXT. See [JSON Data Type](/columns-storage-engines-and-plugins/data-types/string-data-types/json-data-type/) for details.

## Oracle Mode

##### MariaDB starting with [10.3](/kb/en/what-is-mariadb-103/)

In [Oracle mode from MariaDB 10.3](/kb/en/sql_modeoracle-from-mariadb-103/#synonyms-for-basic-sql-types), `CLOB` is a synonym for `LONGTEXT`.

## See Also

- [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text/)
- [BLOB and TEXT Data Types](/columns-storage-engines-and-plugins/data-types/string-data-types/blob-and-text-data-types/)
- [Data Type Storage Requirements](/columns-storage-engines-and-plugins/data-types/data-type-storage-requirements/)
- [JSON Data Type](/columns-storage-engines-and-plugins/data-types/string-data-types/json-data-type/)
- [Oracle mode from MariaDB 10.3](/kb/en/sql_modeoracle-from-mariadb-103/#synonyms-for-basic-sql-types)