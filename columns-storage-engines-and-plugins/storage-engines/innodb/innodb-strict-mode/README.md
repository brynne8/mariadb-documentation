# InnoDB Strict Mode

[InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) strict mode is similar to [SQL strict mode](/kb/en/sql-mode/#strict-mode). When it is enabled, certain InnoDB warnings become errors instead.

## Configuring InnoDB Strict Mode

##### MariaDB starting with [10.2.2](/kb/en/mariadb-1022-release-notes/)

In [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/) and later, InnoDB strict mode is enabled by default.

InnoDB strict mode can be enabled or disabled by configuring the <a undefined>innodb_strict_mode</a> server system variable.

Its global value can be changed dynamically with <a undefined>SET GLOBAL</a>. For example:

```sql
SET GLOBAL innodb_strict_mode=ON;
```

Its value for the current session can also be changed dynamically with <a undefined>SET SESSION</a>. For example:

```sql
SET SESSION innodb_strict_mode=ON;
```

It can also be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files) prior to starting up the server. For example:

```sql
[mariadb]
...
innodb_strict_mode=ON
```

## InnoDB Strict Mode Errors

### Wrong Create Options

If InnoDB strict mode is enabled, and if a DDL statement is executed and invalid or conflicting [table options](/kb/en/create-table/#table-options) are specified, then an error is raised. The error will only be a generic error that says the following:

```sql
ERROR 1005 (HY000): Can't create table `db1`.`tab` (errno: 140 "Wrong create options")
```

However, more details about the error can be found by executing [SHOW WARNINGS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-warnings).

For example, the error is raised in the following cases:

- The <a undefined>KEY_BLOCK_SIZE</a> table option is set to a non-zero value, but the <a undefined>ROW_FORMAT</a> table option is set to some row format other than the [COMPRESSED](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-compressed-row-format) row format. For example:

```sql
SET SESSION innodb_strict_mode=ON;

CREATE OR REPLACE TABLE tab (
   id int PRIMARY KEY,
   str varchar(50)
)
KEY_BLOCK_SIZE=4
ROW_FORMAT=DYNAMIC;
ERROR 1005 (HY000): Can't create table `db1`.`tab` (errno: 140 "Wrong create options")

SHOW WARNINGS;
+---------+------+--------------------------------------------------------------------+
| Level   | Code | Message                                                            |
+---------+------+--------------------------------------------------------------------+
| Warning | 1478 | InnoDB: cannot specify ROW_FORMAT = DYNAMIC with KEY_BLOCK_SIZE.   |
| Error   | 1005 | Can't create table `db1`.`tab` (errno: 140 "Wrong create options") |
| Warning | 1030 | Got error 140 "Wrong create options" from storage engine InnoDB    |
+---------+------+--------------------------------------------------------------------+
3 rows in set (0.000 sec)
```

- The <a undefined>KEY_BLOCK_SIZE</a> table option is set to a non-zero value, but the configured value is larger than either `16` or the value of the <a undefined>innodb_page_size</a> system variable, whichever is smaller.

```sql
SET SESSION innodb_strict_mode=ON;

CREATE OR REPLACE TABLE tab (
   id int PRIMARY KEY,
   str varchar(50)
)
KEY_BLOCK_SIZE=16;
ERROR 1005 (HY000): Can't create table `db1`.`tab` (errno: 140 "Wrong create options")

SHOW WARNINGS;
+---------+------+--------------------------------------------------------------------+
| Level   | Code | Message                                                            |
+---------+------+--------------------------------------------------------------------+
| Warning | 1478 | InnoDB: KEY_BLOCK_SIZE=16 cannot be larger than 8.                 |
| Error   | 1005 | Can't create table `db1`.`tab` (errno: 140 "Wrong create options") |
| Warning | 1030 | Got error 140 "Wrong create options" from storage engine InnoDB    |
+---------+------+--------------------------------------------------------------------+
3 rows in set (0.000 sec)
```

- The <a undefined>KEY_BLOCK_SIZE</a> table option is set to a non-zero value, but the <a undefined>innodb_file_per_table</a> system variable is not set to `ON`.

```sql
SET GLOBAL innodb_file_per_table=OFF;
SET SESSION innodb_strict_mode=ON;

CREATE OR REPLACE TABLE tab (
   id int PRIMARY KEY,
   str varchar(50)
)
KEY_BLOCK_SIZE=4;
ERROR 1005 (HY000): Can't create table `db1`.`tab` (errno: 140 "Wrong create options")

SHOW WARNINGS;
+---------+------+--------------------------------------------------------------------+
| Level   | Code | Message                                                            |
+---------+------+--------------------------------------------------------------------+
| Warning | 1478 | InnoDB: KEY_BLOCK_SIZE requires innodb_file_per_table.             |
| Error   | 1005 | Can't create table `db1`.`tab` (errno: 140 "Wrong create options") |
| Warning | 1030 | Got error 140 "Wrong create options" from storage engine InnoDB    |
+---------+------+--------------------------------------------------------------------+
3 rows in set (0.000 sec)
```

- The <a undefined>KEY_BLOCK_SIZE</a> table option is set to a non-zero value, but it is not set to one of the supported values: [1, 2, 4, 8, 16].

```sql
SET SESSION innodb_strict_mode=ON;

CREATE OR REPLACE TABLE tab (
   id int PRIMARY KEY,
   str varchar(50)
)
KEY_BLOCK_SIZE=5;
ERROR 1005 (HY000): Can't create table `db1`.`tab` (errno: 140 "Wrong create options")

SHOW WARNINGS;
+---------+------+-----------------------------------------------------------------------+
| Level   | Code | Message                                                               |
+---------+------+-----------------------------------------------------------------------+
| Warning | 1478 | InnoDB: invalid KEY_BLOCK_SIZE = 5. Valid values are [1, 2, 4, 8, 16] |
| Error   | 1005 | Can't create table `db1`.`tab` (errno: 140 "Wrong create options")    |
| Warning | 1030 | Got error 140 "Wrong create options" from storage engine InnoDB       |
+---------+------+-----------------------------------------------------------------------+
3 rows in set (0.000 sec)
```

- The <a undefined>ROW_FORMAT</a> table option is set to the [COMPRESSED](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-compressed-row-format) row format, but the <a undefined>innodb_file_per_table</a> system variable is not set to `ON`.

```sql
SET GLOBAL innodb_file_per_table=OFF;
SET SESSION innodb_strict_mode=ON;

CREATE OR REPLACE TABLE tab (
   id int PRIMARY KEY,
   str varchar(50)
)
ROW_FORMAT=COMPRESSED;
ERROR 1005 (HY000): Can't create table `db1`.`tab` (errno: 140 "Wrong create options")

SHOW WARNINGS;
+---------+------+--------------------------------------------------------------------+
| Level   | Code | Message                                                            |
+---------+------+--------------------------------------------------------------------+
| Warning | 1478 | InnoDB: ROW_FORMAT=COMPRESSED requires innodb_file_per_table.      |
| Error   | 1005 | Can't create table `db1`.`tab` (errno: 140 "Wrong create options") |
| Warning | 1030 | Got error 140 "Wrong create options" from storage engine InnoDB    |
+---------+------+--------------------------------------------------------------------+
3 rows in set (0.000 sec)
```

- The <a undefined>ROW_FORMAT</a> table option is set to a value, but it is not set to one of the values supported by InnoDB: [REDUNDANT](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-redundant-row-format), [COMPACT](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-compact-row-format), [DYNAMIC](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-dynamic-row-format), and [COMPRESSED](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-compressed-row-format).

```sql
SET SESSION innodb_strict_mode=ON;

CREATE OR REPLACE TABLE tab (
   id int PRIMARY KEY,
   str varchar(50)
)
ROW_FORMAT=PAGE;
ERROR 1005 (HY000): Can't create table `db1`.`tab` (errno: 140 "Wrong create options")

SHOW WARNINGS;
+---------+------+--------------------------------------------------------------------+
| Level   | Code | Message                                                            |
+---------+------+--------------------------------------------------------------------+
| Warning | 1478 | InnoDB: invalid ROW_FORMAT specifier.                              |
| Error   | 1005 | Can't create table `db1`.`tab` (errno: 140 "Wrong create options") |
| Warning | 1030 | Got error 140 "Wrong create options" from storage engine InnoDB    |
+---------+------+--------------------------------------------------------------------+
3 rows in set (0.000 sec)
```

- Either the <a undefined>KEY_BLOCK_SIZE</a> table option is set to a non-zero value or the <a undefined>ROW_FORMAT</a> table option is set to the [COMPRESSED](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-compressed-row-format) row format, but the <a undefined>innodb_page_size</a> system variable is set to a value greater than `16k`.

```sql
SET SESSION innodb_strict_mode=ON;

CREATE OR REPLACE TABLE tab (
   id int PRIMARY KEY,
   str varchar(50)
)
ROW_FORMAT=COMPRESSED;
ERROR 1005 (HY000): Can't create table `db1`.`tab` (errno: 140 "Wrong create options")

SHOW WARNINGS;
+---------+------+-----------------------------------------------------------------------+
| Level   | Code | Message                                                               |
+---------+------+-----------------------------------------------------------------------+
| Warning | 1478 | InnoDB: Cannot create a COMPRESSED table when innodb_page_size > 16k. |
| Error   | 1005 | Can't create table `db1`.`tab` (errno: 140 "Wrong create options")    |
| Warning | 1030 | Got error 140 "Wrong create options" from storage engine InnoDB       |
+---------+------+-----------------------------------------------------------------------+
3 rows in set (0.00 sec)
```

- The <a undefined>DATA DIRECTORY</a> table option is set, but the <a undefined>innodb_file_per_table</a> system variable is not set to `ON`.

```sql
SET GLOBAL innodb_file_per_table=OFF;
SET SESSION innodb_strict_mode=ON;

CREATE OR REPLACE TABLE tab (
   id int PRIMARY KEY,
   str varchar(50)
)
DATA DIRECTORY='/mariadb';
ERROR 1005 (HY000): Can't create table `db1`.`tab` (errno: 140 "Wrong create options")

SHOW WARNINGS;
+---------+------+--------------------------------------------------------------------+
| Level   | Code | Message                                                            |
+---------+------+--------------------------------------------------------------------+
| Warning | 1478 | InnoDB: DATA DIRECTORY requires innodb_file_per_table.             |
| Error   | 1005 | Can't create table `db1`.`tab` (errno: 140 "Wrong create options") |
| Warning | 1030 | Got error 140 "Wrong create options" from storage engine InnoDB    |
+---------+------+--------------------------------------------------------------------+
3 rows in set (0.000 sec)
```

- The <a undefined>DATA DIRECTORY</a> table option is set, but the table is a [temporary table](/kb/en/create-table/#create-temporary-table).

```sql
SET SESSION innodb_strict_mode=ON;

CREATE OR REPLACE TEMPORARY TABLE tab (
   id int PRIMARY KEY,
   str varchar(50)
)
DATA DIRECTORY='/mariadb';
ERROR 1005 (HY000): Can't create table `db1`.`tab` (errno: 140 "Wrong create options")

SHOW WARNINGS;
+---------+------+--------------------------------------------------------------------+
| Level   | Code | Message                                                            |
+---------+------+--------------------------------------------------------------------+
| Warning | 1478 | InnoDB: DATA DIRECTORY cannot be used for TEMPORARY tables.        |
| Error   | 1005 | Can't create table `db1`.`tab` (errno: 140 "Wrong create options") |
| Warning | 1030 | Got error 140 "Wrong create options" from storage engine InnoDB    |
+---------+------+--------------------------------------------------------------------+
3 rows in set (0.000 sec)
```

- The <a undefined>INDEX DIRECTORY</a> table option is set.

```sql
SET SESSION innodb_strict_mode=ON;

CREATE OR REPLACE TABLE tab (
   id int PRIMARY KEY,
   str varchar(50)
)
INDEX DIRECTORY='/mariadb';
ERROR 1005 (HY000): Can't create table `db1`.`tab` (errno: 140 "Wrong create options")

SHOW WARNINGS;
+---------+------+--------------------------------------------------------------------+
| Level   | Code | Message                                                            |
+---------+------+--------------------------------------------------------------------+
| Warning | 1478 | InnoDB: INDEX DIRECTORY is not supported                           |
| Error   | 1005 | Can't create table `db1`.`tab` (errno: 140 "Wrong create options") |
| Warning | 1030 | Got error 140 "Wrong create options" from storage engine InnoDB    |
+---------+------+--------------------------------------------------------------------+
3 rows in set (0.000 sec)
```

- The <a undefined>PAGE_COMPRESSED</a> table option is set to `1`, so [InnoDB page compression](/kb/en/compression/) is enabled, but the <a undefined>ROW_FORMAT</a> table option is set to some row format other than the [COMPACT](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-compact-row-format) or [DYNAMIC](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-dynamic-row-format) row formats.

```sql
SET SESSION innodb_strict_mode=ON;

CREATE OR REPLACE TABLE tab (
   id int PRIMARY KEY,
   str varchar(50)
)
PAGE_COMPRESSED=1
ROW_FORMAT=COMPRESSED;
ERROR 1005 (HY000): Can't create table `db1`.`tab` (errno: 140 "Wrong create options")

SHOW WARNINGS;
+---------+------+--------------------------------------------------------------------+
| Level   | Code | Message                                                            |
+---------+------+--------------------------------------------------------------------+
| Warning |  140 | InnoDB: PAGE_COMPRESSED table can't have ROW_TYPE=COMPRESSED       |
| Error   | 1005 | Can't create table `db1`.`tab` (errno: 140 "Wrong create options") |
| Warning | 1030 | Got error 140 "Wrong create options" from storage engine InnoDB    |
+---------+------+--------------------------------------------------------------------+
3 rows in set (0.000 sec)
```

- The <a undefined>PAGE_COMPRESSED</a> table option is set to `1`, so [InnoDB page compression](/kb/en/compression/) is enabled, but the <a undefined>innodb_file_per_table</a> system variable is not set to `ON`.

```sql
SET GLOBAL innodb_file_per_table=OFF;
SET SESSION innodb_strict_mode=ON;

CREATE OR REPLACE TABLE tab (
   id int PRIMARY KEY,
   str varchar(50)
)
PAGE_COMPRESSED=1;
ERROR 1005 (HY000): Can't create table `db1`.`tab` (errno: 140 "Wrong create options")

SHOW WARNINGS;
+---------+------+--------------------------------------------------------------------+
| Level   | Code | Message                                                            |
+---------+------+--------------------------------------------------------------------+
| Warning |  140 | InnoDB: PAGE_COMPRESSED requires innodb_file_per_table.            |
| Error   | 1005 | Can't create table `db1`.`tab` (errno: 140 "Wrong create options") |
| Warning | 1030 | Got error 140 "Wrong create options" from storage engine InnoDB    |
+---------+------+--------------------------------------------------------------------+
3 rows in set (0.000 sec)
```

- The <a undefined>PAGE_COMPRESSED</a> table option is set to `1`, so [InnoDB page compression](/kb/en/compression/) is enabled, but the <a undefined>KEY_BLOCK_SIZE</a> table option is also specified.

```sql
SET SESSION innodb_strict_mode=ON;

CREATE OR REPLACE TABLE tab (
   id int PRIMARY KEY,
   str varchar(50)
)
PAGE_COMPRESSED=1
KEY_BLOCK_SIZE=4;
ERROR 1005 (HY000): Can't create table `db1`.`tab` (errno: 140 "Wrong create options")

SHOW WARNINGS;
+---------+------+--------------------------------------------------------------------+
| Level   | Code | Message                                                            |
+---------+------+--------------------------------------------------------------------+
| Warning |  140 | InnoDB: PAGE_COMPRESSED table can't have key_block_size            |
| Error   | 1005 | Can't create table `db1`.`tab` (errno: 140 "Wrong create options") |
| Warning | 1030 | Got error 140 "Wrong create options" from storage engine InnoDB    |
+---------+------+--------------------------------------------------------------------+
3 rows in set (0.000 sec)
```

- The <a undefined>PAGE_COMPRESSION_LEVEL</a> table option is set, but 
 the <a undefined>PAGE_COMPRESSED</a> table option is set to `0`, so [InnoDB page compression](/kb/en/compression/) is disabled.

```sql
SET SESSION innodb_strict_mode=ON;

CREATE OR REPLACE TABLE tab (
   id int PRIMARY KEY,
   str varchar(50)
)
PAGE_COMPRESSED=0
PAGE_COMPRESSION_LEVEL=9;
ERROR 1005 (HY000): Can't create table `db1`.`tab` (errno: 140 "Wrong create options")

SHOW WARNINGS;
+---------+------+--------------------------------------------------------------------+
| Level   | Code | Message                                                            |
+---------+------+--------------------------------------------------------------------+
| Warning |  140 | InnoDB: PAGE_COMPRESSION_LEVEL requires PAGE_COMPRESSED            |
| Error   | 1005 | Can't create table `db1`.`tab` (errno: 140 "Wrong create options") |
| Warning | 1030 | Got error 140 "Wrong create options" from storage engine InnoDB    |
+---------+------+--------------------------------------------------------------------+
3 rows in set (0.000 sec)
```

##### MariaDB until [10.2](/kb/en/what-is-mariadb-102/)

In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and before, the error is raised in the following additional cases:

- The <a undefined>KEY_BLOCK_SIZE</a> table option is set to a non-zero value, but the <a undefined>innodb_file_format</a> system variable is not set to `Barracuda`.

```sql
SET GLOBAL innodb_file_format='Antelope';
SET SESSION innodb_strict_mode=ON;

CREATE OR REPLACE TABLE tab (
   id int PRIMARY KEY,
   str varchar(50)
)
KEY_BLOCK_SIZE=4;
ERROR 1005 (HY000): Can't create table `db1`.`tab` (errno: 140 "Wrong create options")

SHOW WARNINGS;
+---------+------+--------------------------------------------------------------------+
| Level   | Code | Message                                                            |
+---------+------+--------------------------------------------------------------------+
| Warning | 1478 | InnoDB: KEY_BLOCK_SIZE requires innodb_file_format > Antelope.     |
| Error   | 1005 | Can't create table `db1`.`tab` (errno: 140 "Wrong create options") |
| Warning | 1030 | Got error 140 "Wrong create options" from storage engine InnoDB    |
+---------+------+--------------------------------------------------------------------+
3 rows in set (0.00 sec)
```

- The <a undefined>ROW_FORMAT</a> table option is set to either the [COMPRESSED](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-compressed-row-format) or the [DYNAMIC](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-dynamic-row-format) row format, but the <a undefined>innodb_file_format</a> system variable is not set to `Barracuda`.

```sql
SET GLOBAL innodb_file_format='Antelope';
SET SESSION innodb_strict_mode=ON;

CREATE OR REPLACE TABLE tab (
   id int PRIMARY KEY,
   str varchar(50)
)
ROW_FORMAT=COMPRESSED;
ERROR 1005 (HY000): Can't create table `db1`.`tab` (errno: 140 "Wrong create options")

SHOW WARNINGS;
+---------+------+-----------------------------------------------------------------------+
| Level   | Code | Message                                                               |
+---------+------+-----------------------------------------------------------------------+
| Warning | 1478 | InnoDB: ROW_FORMAT=COMPRESSED requires innodb_file_format > Antelope. |
| Error   | 1005 | Can't create table `db1`.`tab` (errno: 140 "Wrong create options")    |
| Warning | 1030 | Got error 140 "Wrong create options" from storage engine InnoDB       |
+---------+------+-----------------------------------------------------------------------+
3 rows in set (0.00 sec)
```

- The <a undefined>PAGE_COMPRESSED</a> table option is set to `1`, so [InnoDB page compression](/kb/en/compression/) is enabled, but the <a undefined>innodb_file_format</a> system variable is not set to `Barracuda`.

```sql
SET GLOBAL innodb_file_format='Antelope';
SET SESSION innodb_strict_mode=ON;

CREATE OR REPLACE TABLE tab (
   id int PRIMARY KEY,
   str varchar(50)
)
PAGE_COMPRESSED=1;
SHOW WARNINGS;
ERROR 1005 (HY000): Can't create table `db1`.`tab` (errno: 140 "Wrong create options")

SHOW WARNINGS;
+---------+------+--------------------------------------------------------------------+
| Level   | Code | Message                                                            |
+---------+------+--------------------------------------------------------------------+
| Warning |  140 | InnoDB: PAGE_COMPRESSED requires innodb_file_format > Antelope.    |
| Error   | 1005 | Can't create table `db1`.`tab` (errno: 140 "Wrong create options") |
| Warning | 1030 | Got error 140 "Wrong create options" from storage engine InnoDB    |
+---------+------+--------------------------------------------------------------------+
3 rows in set (0.00 sec)
```

### COMPRESSED Row Format

If InnoDB strict mode is enabled, and if a table uses the [COMPRESSED](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-compressed-row-format) row format, and if the table's <a undefined>KEY_BLOCK_SIZE</a> is too small to contain a row, then an error is returned by the statement.

### Row Size Too Large

If InnoDB strict mode is enabled, and if a table exceeds its row format's [maximum row size](/kb/en/innodb-row-formats-overview/#maximum-row-size), then InnoDB will return an error.

```sql
ERROR 1118 (42000): Row size too large (> 8126). Changing some columns to 
TEXT or BLOB may help. In current row format, BLOB prefix of 0 bytes is stored inline.
```

See [Troubleshooting Row Size Too Large Errors with InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/troubleshooting-row-size-too-large-errors-with-innodb) for more information.