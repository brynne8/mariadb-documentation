# NOW

## Syntax

```sql
NOW([precision])
CURRENT_TIMESTAMP
CURRENT_TIMESTAMP([precision])
LOCALTIME, LOCALTIME([precision])
LOCALTIMESTAMP
LOCALTIMESTAMP([precision])
```

## Description

Returns the current date and time as a value in `'YYYY-MM-DD HH:MM:SS'`
or `YYYYMMDDHHMMSS.uuuuuu` format, depending on whether the function is
used in a string or numeric context. The value is expressed in the
current [time zone](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/time-zones).

The optional <em>precision</em> determines the microsecond precision. See [Microseconds in MariaDB](/built-in-functions/date-time-functions/microseconds-in-mariadb).

`NOW()` (or its synonyms) can be used as the default value for [TIMESTAMP](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/timestamp) columns as well as, since [MariaDB 10.0.1](/kb/en/mariadb-1001-release-notes/), [DATETIME](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/datetime) columns. Before [MariaDB 10.0.1](/kb/en/mariadb-1001-release-notes/), it was only possible for a single TIMESTAMP column per table to contain the CURRENT_TIMESTAMP as its default.

When displayed in the [INFORMATION_SCHEMA.COLUMNS](/kb/en/information-schema-columns-table/) table, a default [CURRENT TIMESTAMP](/built-in-functions/date-time-functions/current_timestamp) is displayed as `CURRENT_TIMESTAMP` up until [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/), and as `current_timestamp()` from [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/), due to to [MariaDB 10.2](/kb/en/what-is-mariadb-102/) accepting expressions in the DEFAULT clause.

## Examples

```sql
SELECT NOW();
+---------------------+
| NOW()               |
+---------------------+
| 2010-03-27 13:13:25 |
+---------------------+

SELECT NOW() + 0;
+-----------------------+
| NOW() + 0             |
+-----------------------+
| 20100327131329.000000 |
+-----------------------+
```

With precision:

```sql
SELECT CURRENT_TIMESTAMP(2);
+------------------------+
| CURRENT_TIMESTAMP(2)   |
+------------------------+
| 2018-07-10 09:47:26.24 |
+------------------------+
```

Used as a default TIMESTAMP:

```sql
CREATE TABLE t (createdTS TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP);
```

From [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/):

```sql
SELECT * FROM INFORMATION_SCHEMA.COLUMNS WHERE TABLE_SCHEMA='test'
  AND COLUMN_NAME LIKE '%ts%'\G
*************************** 1. row ***************************
           TABLE_CATALOG: def
            TABLE_SCHEMA: test
              TABLE_NAME: t
             COLUMN_NAME: ts
        ORDINAL_POSITION: 1
          COLUMN_DEFAULT: current_timestamp()
...
```

&lt;= [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/)

```sql
SELECT * FROM INFORMATION_SCHEMA.COLUMNS WHERE TABLE_SCHEMA='test'
  AND COLUMN_NAME LIKE '%ts%'\G
*************************** 1. row ***************************
           TABLE_CATALOG: def
            TABLE_SCHEMA: test
              TABLE_NAME: t
             COLUMN_NAME: createdTS
        ORDINAL_POSITION: 1
          COLUMN_DEFAULT: CURRENT_TIMESTAMP
...
```

## See Also

- [Microseconds in MariaDB](/built-in-functions/date-time-functions/microseconds-in-mariadb)
- [timestamp server system variable](/kb/en/server-system-variables/#timestamp)