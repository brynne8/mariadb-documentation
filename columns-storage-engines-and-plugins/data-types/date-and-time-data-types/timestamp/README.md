# TIMESTAMP

## Syntax

```sql
TIMESTAMP [(<microsecond precision)]
```

## Description

A timestamp in the format `YYYY-MM-DD HH:MM:SS.ffffff`.

The timestamp field is generally used to define at which moment in time a row was added or updated and by default will automatically be assigned the current datetime when a record is inserted or updated. The automatic properties only apply to the first TIMESTAMP in the record; subsequent TIMESTAMP columns will not be changed.

##### MariaDB starting with [10.1.2](/kb/en/mariadb-1012-release-notes/)

[MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/) introduced the [--mysql56-temporal-format](/kb/en/server-system-variables/#mysql56_temporal_format) option, on by default, which allows MariaDB to store TIMESTAMPs using the same low-level format MySQL 5.6 uses.

For more information, see [Internal Format](#internal-format).

## Supported Values

MariaDB stores values that use the `TIMESTAMP` data type as the number of seconds since '1970-01-01 00:00:00' ([UTC](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/coordinated-universal-time)). This means that the `TIMESTAMP` data type can hold values between '1970-01-01 00:00:01' ([UTC](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/coordinated-universal-time)) and '2038-01-19 03:14:07' ([UTC](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/coordinated-universal-time)).

MariaDB can also store [microseconds](/built-in-functions/date-time-functions/microseconds-in-mariadb) with a precision between 0 and 6. If no microsecond precision is specified, then 0 is used by default.

## Automatic Values

MariaDB has special behavior for the first column that uses the `TIMESTAMP` data type in a specific table.  For the first column that uses the `TIMESTAMP` data type in a specific table, MariaDB automatically assigns the following properties to the column:

- `DEFAULT CURRENT_TIMESTAMP`
- `ON UPDATE CURRENT_TIMESTAMP`

This means that if the column is not explicitly assigned a value in an `INSERT` or `UPDATE` query, then MariaDB will automatically initialize the column's value with the current date and time.

This automatic initialization for `INSERT` and `UPDATE` queries can also be <strong>explicitly enabled</strong> for a column that uses the `TIMESTAMP` data type by specifying the `DEFAULT CURRENT_TIMESTAMP` and `ON UPDATE CURRENT_TIMESTAMP` clauses for the column. In these clauses, any synonym of [CURRENT_TIMESTAMP](/built-in-functions/date-time-functions/now) is accepted, including `CURRENT_TIMESTAMP()`, `NOW()`, `LOCALTIME`, `LOCALTIME()`, `LOCALTIMESTAMP`, and `LOCALTIMESTAMP()`.

This automatic initialization for `INSERT` queries can also be <strong>explicitly disabled</strong> for a column that uses the `TIMESTAMP` data type by specifying a constant `DEFAULT` value. For example, `DEFAULT 0`.

This automatic initialization for `UPDATE` queries can also be <strong>explicitly disabled</strong> for a column that uses the `TIMESTAMP` data type by specifying a `DEFAULT` clause for the column, but no `ON UPDATE` clause. If a `DEFAULT` clause is explicitly specified for a column that uses the `TIMESTAMP` data type, but an `ON UPDATE` clause is not specified for the column, then the timestamp value will not automatically change when an `UPDATE` statement is executed.

MariaDB also has special behavior if `NULL` is assigned to column that uses the `TIMESTAMP` data type. If the column is assigned the `NULL` value in an `INSERT` or `UPDATE` query, then MariaDB will automatically initialize the column's value with the current date and time. For details, see [NULL values in MariaDB](/kb/en/null-values-in-mariadb/#auto_increment-timestamp-and-virtual-columns).

This automatic initialization for `NULL` values can also be <strong>explicitly disabled</strong> for a column that uses the `TIMESTAMP` data type by specifying the `NULL` attribute for the column. In this case, if the column's value is set to `NULL`, then the column's value will actually be set to `NULL`.

## Time Zones

If a column uses the `TIMESTAMP` data type, then any inserted values are converted from the session's time zone to [Coordinated Universal Time (UTC)](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/coordinated-universal-time) when stored, and converted back to the session's time zone when retrieved.

MariaDB does not currently store any time zone identifier with the value of the `TIMESTAMP` data type. See [MDEV-10018](https://jira.mariadb.org/browse/MDEV-10018) for more information.

MariaDB does not currently support time zone literals that contain time zone identifiers. See [MDEV-11829](https://jira.mariadb.org/browse/MDEV-11829) for more information.

## Limitations

- Because the TIMESTAMP value is stored as Epoch Seconds, the timestamp value '1970-01-01 00:00:00' (UTC) is reserved since the second #0 is used to represent '0000-00-00 00:00:00'.
- In [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and before there could only be one TIMESTAMP column per table that had CURRENT_TIMESTAMP defined as its default value. This limit has no longer applied since [MariaDB 10.0](/kb/en/what-is-mariadb-100/).

## SQL_MODE=MAXDB

If the [SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode) is set to `MAXDB`, TIMESTAMP fields will be silently converted to [DATETIME](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/datetime).

## Internal Format

In [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/) a new temporal format was introduced from MySQL 5.6 that alters how the `TIME`, `DATETIME` and `TIMESTAMP` columns operate at lower levels.  These changes allow these temporal data types to have fractional parts and negative values.  You can disable this feature using the <a undefined>mysql56_temporal_format</a> system variable.

Tables that include `TIMESTAMP` values that were created on an older version of MariaDB or that were created while the <a undefined>mysql56_temporal_format</a> system variable was disabled continue to store data using the older data type format.

In order to update table columns from the older format to the newer format, execute an <a undefined>ALTER TABLE... MODIFY COLUMN</a> statement that changes the column to the *same* data type. This change may be needed if you want to export the table's tablespace and import it onto a server that has `mysql56_temporal_format=ON` set (see [MDEV-15225](https://jira.mariadb.org/browse/MDEV-15225)).

For instance, if you have a `TIMESTAMP` column in your table:

```sql
SHOW VARIABLES LIKE 'mysql56_temporal_format';

+-------------------------+-------+
| Variable_name           | Value |
+-------------------------+-------+
| mysql56_temporal_format | ON    |
+-------------------------+-------+

ALTER TABLE example_table MODIFY ts_col TIMESTAMP;
```

When MariaDB executes the [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table) statement, it converts the data from the older temporal format to the newer one.

In the event that you have several tables and columns using temporal data types that you want to switch over to the new format, make sure the system variable is enabled, then perform a dump and restore using `mysqldump`.  The columns using relevant temporal data types are restored using the new temporal format.

Starting from [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/) columns with old temporal formats are marked with a `/* mariadb-5.3 */`  comment in the output of [SHOW CREATE TABLE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-table), [SHOW COLUMNS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-columns), [DESCRIBE](/sql-statements-structure/sql-statements/administrative-sql-statements/describe) statements, as well as in the `COLUMN_TYPE` column of the <a undefined>INFORMATION_SCHEMA.COLUMNS Table</a>.

```sql
SHOW CREATE TABLE mariadb5312_timestamp\G
*************************** 1. row ***************************
       Table: mariadb5312_timestamp
Create Table: CREATE TABLE `mariadb5312_timestamp` (
  `ts0` timestamp /* mariadb-5.3 */ NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp(),
  `ts6` timestamp(6) /* mariadb-5.3 */ NOT NULL DEFAULT '0000-00-00 00:00:00.000000'
) ENGINE=MyISAM DEFAULT CHARSET=latin1
```

<strong>Note:</strong>  Prior to MySQL 4.1 a different format for the TIMESTAMP datatype was used. This format is unsupported in [MariaDB 5.1](/kb/en/what-is-mariadb-51/) and upwards.

## Examples

```sql
CREATE TABLE t (id INT, ts TIMESTAMP);

DESC t;
+-------+-----------+------+-----+-------------------+-----------------------------+
| Field | Type      | Null | Key | Default           | Extra                       |
+-------+-----------+------+-----+-------------------+-----------------------------+
| id    | int(11)   | YES  |     | NULL              |                             |
| ts    | timestamp | NO   |     | CURRENT_TIMESTAMP | on update CURRENT_TIMESTAMP |
+-------+-----------+------+-----+-------------------+-----------------------------+

INSERT INTO t(id)  VALUES (1),(2);

SELECT * FROM t;
+------+---------------------+
| id   | ts                  |
+------+---------------------+
|    1 | 2013-07-22 12:50:05 |
|    2 | 2013-07-22 12:50:05 |
+------+---------------------+

INSERT INTO t  VALUES (3,NULL),(4,'2001-07-22 12:12:12');

SELECT * FROM t;
+------+---------------------+
| id   | ts                  |
+------+---------------------+
|    1 | 2013-07-22 12:50:05 |
|    2 | 2013-07-22 12:50:05 |
|    3 | 2013-07-22 12:51:56 |
|    4 | 2001-07-22 12:12:12 |
+------+---------------------+
```

Converting to Unix epoch:

```sql
SELECT ts, UNIX_TIMESTAMP(ts) FROM t;
+---------------------+--------------------+
| ts                  | UNIX_TIMESTAMP(ts) |
+---------------------+--------------------+
| 2013-07-22 12:50:05 |         1374490205 |
| 2013-07-22 12:50:05 |         1374490205 |
| 2013-07-22 12:51:56 |         1374490316 |
| 2001-07-22 12:12:12 |          995796732 |
+---------------------+--------------------+
```

Update also changes the timestamp:

```sql
UPDATE t set id=5 WHERE id=1;

SELECT * FROM t;
+------+---------------------+
| id   | ts                  |
+------+---------------------+
|    5 | 2013-07-22 14:52:33 |
|    2 | 2013-07-22 12:50:05 |
|    3 | 2013-07-22 12:51:56 |
|    4 | 2001-07-22 12:12:12 |
+------+---------------------+
```

Default NULL:

```sql
CREATE TABLE t2 (id INT, ts TIMESTAMP NULL ON UPDATE CURRENT_TIMESTAMP);

INSERT INTO t(id)  VALUES (1),(2);

SELECT * FROM t2;

INSERT INTO t2(id)  VALUES (1),(2);

SELECT * FROM t2;
+------+------+
| id   | ts   |
+------+------+
|    1 | NULL |
|    2 | NULL |
+------+------+

UPDATE t2 SET id=3 WHERE id=1;

SELECT * FROM t2;
+------+---------------------+
| id   | ts                  |
+------+---------------------+
|    3 | 2013-07-22 15:32:22 |
|    2 | NULL                |
+------+---------------------+
```

Only the first timestamp is automatically inserted and updated:

```sql
CREATE TABLE t3 (id INT, ts1 TIMESTAMP, ts2 TIMESTAMP);

INSERT INTO t3(id)  VALUES (1),(2);

SELECT * FROM t3;
+------+---------------------+---------------------+
| id   | ts1                 | ts2                 |
+------+---------------------+---------------------+
|    1 | 2013-07-22 15:35:07 | 0000-00-00 00:00:00 |
|    2 | 2013-07-22 15:35:07 | 0000-00-00 00:00:00 |
+------+---------------------+---------------------+

DESC t3;
+-------+-----------+------+-----+---------------------+-----------------------------+
| Field | Type      | Null | Key | Default             | Extra                       |
+-------+-----------+------+-----+---------------------+-----------------------------+
| id    | int(11)   | YES  |     | NULL                |                             |
| ts1   | timestamp | NO   |     | CURRENT_TIMESTAMP   | on update CURRENT_TIMESTAMP |
| ts2   | timestamp | NO   |     | 0000-00-00 00:00:00 |                             |
+-------+-----------+------+-----+---------------------+-----------------------------+
```

Explicitly setting a timestamp with the [CURRENT_TIMESTAMP](/built-in-functions/date-time-functions/current_timestamp) function:

```sql
INSERT INTO t3(id,ts2)  VALUES (3,CURRENT_TIMESTAMP());

SELECT * FROM t3;
+------+---------------------+---------------------+
| id   | ts1                 | ts2                 |
+------+---------------------+---------------------+
|    1 | 2013-07-22 15:35:07 | 0000-00-00 00:00:00 |
|    2 | 2013-07-22 15:35:07 | 0000-00-00 00:00:00 |
|    3 | 2013-07-22 15:38:52 | 2013-07-22 15:38:52 |
+------+---------------------+---------------------+
```

Specifying the timestamp as NOT NULL:

```sql
CREATE TABLE t4 (id INT, ts TIMESTAMP NOT NULL);

INSERT INTO t4(id)  VALUES (1);
SELECT SLEEP(1);
INSERT INTO t4(id,ts) VALUES (2,NULL);

SELECT * FROM t4;
```

## See Also

- [Data Type Storage Requirements](/columns-storage-engines-and-plugins/data-types/data-type-storage-requirements)