# DATE

## Syntax

```sql
DATE
```

## Description

A date. The supported range is '`1000-01-01`' to '`9999-12-31`'. MariaDB
displays `DATE` values in '`YYYY-MM-DD`' format, but can be assigned dates in looser formats, including strings or numbers, as long as they make sense. These include a short year, `YY-MM-DD`, no delimiters, `YYMMDD`, or any other acceptable delimiter, for example `YYYY/MM/DD`. For details, see [date and time literals](/sql-statements-structure/sql-language-structure/date-and-time-literals).

'`0000-00-00`' is a permitted special value (zero-date), unless the [NO_ZERO_DATE](/kb/en/sql-mode/#no_zero_date) [SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode) is used. Also, individual components of a date can be set to 0 (for example: '`2015-00-12`'), unless the [NO_ZERO_IN_DATE](/kb/en/sql-mode/#no_zero_in_date) [SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode) is used. In many cases, the result of en expression involving a zero-date, or a date with zero-parts, is `NULL`. If the [ALLOW_INVALID_DATES](/kb/en/sql-mode/#allow_invalid_dates) SQL_MODE is enabled, if the day part is in the range between 1 and 31, the date does not produce any error, even for months that have less than 31 days.

### Oracle Mode

##### MariaDB starting with [10.3](/kb/en/what-is-mariadb-103/)

In [Oracle mode from MariaDB 10.3](/kb/en/sql_modeoracle-from-mariadb-103/#synonyms-for-basic-sql-types), `DATE` with a time portion is a synonym for [DATETIME](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/datetime). See also [mariadb_schema](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/mariadb_schema).

## Examples

```sql
CREATE TABLE t1 (d DATE);

INSERT INTO t1 VALUES ("2010-01-12"), ("2011-2-28"), ('120314'),('13*04*21');

SELECT * FROM t1;
+------------+
| d          |
+------------+
| 2010-01-12 |
| 2011-02-28 |
| 2012-03-14 |
| 2013-04-21 |
+------------+
```

## See Also

- [mariadb_schema](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/mariadb_schema) data type qualifier