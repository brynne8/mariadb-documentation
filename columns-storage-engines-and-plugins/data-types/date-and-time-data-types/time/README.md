# TIME

## Syntax

```sql
TIME [(<microsecond precision>)]
```

## Description

A time. The range is `'-838:59:59.999999'` to `'838:59:59.999999'`. [Microsecond precision](/built-in-functions/date-time-functions/microseconds-in-mariadb) can be from 0-6; if not specified 0 is used. Microseconds have been available since [MariaDB 5.3](/kb/en/what-is-mariadb-53/).

MariaDB displays `TIME` values in `'HH:MM:SS.ssssss'` format, but allows assignment of times in looser formats, including 'D HH:MM:SS', 'HH:MM:SS', 'HH:MM', 'D HH:MM', 'D HH', 'SS', or 'HHMMSS', as well as permitting dropping of any leading zeros when a delimiter is provided, for example '3:9:10'. For details, see [date and time literals](/sql-statements-structure/sql-language-structure/date-and-time-literals).

##### MariaDB starting with [10.1.2](/kb/en/mariadb-1012-release-notes/)

[MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/) introduced the [--mysql56-temporal-format](/kb/en/server-system-variables/#mysql56_temporal_format) option, on by default, which allows MariaDB to store TIMEs using the same low-level format MySQL 5.6 uses.

### Internal Format

In [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/) a new temporal format was introduced from MySQL 5.6 that alters how the `TIME`, `DATETIME` and `TIMESTAMP` columns operate at lower levels.  These changes allow these temporal data types to have fractional parts and negative values.  You can disable this feature using the <a undefined>mysql56_temporal_format</a> system variable.

Tables that include `TIMESTAMP` values that were created on an older version of MariaDB or that were created while the <a undefined>mysql56_temporal_format</a> system variable was disabled continue to store data using the older data type format.

In order to update table columns from the older format to the newer format, execute an <a undefined>ALTER TABLE... MODIFY COLUMN</a> statement that changes the column to the *same* data type. This change may be needed if you want to export the table's tablespace and import it onto a server that has `mysql56_temporal_format=ON` set (see [MDEV-15225](https://jira.mariadb.org/browse/MDEV-15225)).

For instance, if you have a `TIME` column in your table:

```sql
SHOW VARIABLES LIKE 'mysql56_temporal_format';

+-------------------------+-------+
| Variable_name           | Value |
+-------------------------+-------+
| mysql56_temporal_format | ON    |
+-------------------------+-------+

ALTER TABLE example_table MODIFY ts_col TIME;
```

When MariaDB executes the [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table) statement, it converts the data from the older temporal format to the newer one.

In the event that you have several tables and columns using temporal data types that you want to switch over to the new format, make sure the system variable is enabled, then perform a dump and restore using `mysqldump`.  The columns using relevant temporal data types are restored using the new temporal format.

Starting from [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/) columns with old temporal formats are marked with a `/* mariadb-5.3 */` comment in the output of [SHOW CREATE TABLE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-table), [SHOW COLUMNS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-columns), [DESCRIBE](/sql-statements-structure/sql-statements/administrative-sql-statements/describe) statements, as well as in the `COLUMN_TYPE` column of the <a undefined>INFORMATION_SCHEMA.COLUMNS Table</a>.

```sql
SHOW CREATE TABLE mariadb5312_time\G
*************************** 1. row ***************************
       Table: mariadb5312_time
Create Table: CREATE TABLE `mariadb5312_time` (
  `t0` time /* mariadb-5.3 */ DEFAULT NULL,
  `t6` time(6) /* mariadb-5.3 */ DEFAULT NULL
) ENGINE=MyISAM DEFAULT CHARSET=latin1
```

Note, columns with the current format are not marked with a comment.

## Examples

```sql
INSERT INTO time VALUES ('90:00:00'), ('800:00:00'), (800), (22), (151413), ('9:6:3'), ('12 09');

SELECT * FROM time;
+-----------+
| t         |
+-----------+
| 90:00:00  |
| 800:00:00 |
| 00:08:00  |
| 00:00:22  |
| 15:14:13  |
| 09:06:03  |
| 297:00:00 |
+-----------+
```

## See also

- [Data Type Storage Requirements](/columns-storage-engines-and-plugins/data-types/data-type-storage-requirements)