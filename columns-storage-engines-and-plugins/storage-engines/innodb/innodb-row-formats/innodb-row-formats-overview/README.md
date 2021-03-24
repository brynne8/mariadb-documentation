# InnoDB Row Formats Overview

The [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) storage engine supports four different row formats:

- [REDUNDANT](#redundant-row-format)
- [COMPACT](#compact-row-format)
- [DYNAMIC](#dynamic-row-format)
- [COMPRESSED](#compressed-row-format)

In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and before, the latter two row formats are only supported if the [InnoDB file format](/kb/en/xtradbinnodb-file-format/) is `Barracuda`. Therefore, the [innodb_file_format](/kb/en/innodb-system-variables/#innodb_file_format) system variable must be set to `Barracuda` to use these row formats in those versions.

In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and before, the latter two row formats are also only supported if the table is in a [file per-table](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-file-per-table-tablespaces/) tablespace. Therefore, the [innodb_file_per_table](/kb/en/innodb-system-variables/#innodb_file_per_table) system variable must be set to `ON` to use these row formats in those versions.

## Default Row Format

##### MariaDB starting with [10.2.2](/kb/en/mariadb-1022-release-notes/)

In [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/) and later, the [innodb_default_row_format](/kb/en/innodb-system-variables/#innodb_default_row_format) system variable can be used to set the default row format for InnoDB tables. The possible values are:

- `redundant`
- `compact`
- `dynamic`

This system variable's default value is `dynamic`, which means that the default row format is `DYNAMIC`.

This system variable cannot be set to `compressed`, which means that the default row format cannot be `COMPRESSED`.

For example, the following statements would create a table with the `DYNAMIC` row format:

```sql
SET SESSION innodb_strict_mode=ON;

SET GLOBAL innodb_default_row_format='dynamic';

CREATE TABLE tab (
   id int,
   str varchar(50)
) ENGINE=InnoDB;
```

##### MariaDB until [10.2.1](/kb/en/mariadb-1021-release-notes/)

In [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/) and before, the default row format is `COMPACT`.

For example, the following statements would create a table with the `COMPACT` row format:

```sql
SET SESSION innodb_strict_mode=ON;

CREATE TABLE tab (
   id int,
   str varchar(50)
) ENGINE=InnoDB;
```

## Setting a Table's Row Format

One way to specify an InnoDB table's row format is by setting the [ROW_FORMAT](/kb/en/create-table/#row_format) table option to the relevant row format in a [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/) or [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/) statement. For example:

```sql
SET SESSION innodb_strict_mode=ON;

SET GLOBAL innodb_file_per_table=ON;

SET GLOBAL innodb_file_format='Barracuda';

CREATE TABLE tab (
   id int,
   str varchar(50)
) ENGINE=InnoDB ROW_FORMAT=DYNAMIC;
```

In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and before, InnoDB can silently ignore and override some row format choices if you do not  have the [innodb_file_format](/kb/en/innodb-system-variables/#innodb_file_format) system variable set to `Barracuda` and the [innodb_file_per_table](/kb/en/innodb-system-variables/#innodb_file_per_table) system variable set to `ON`.

## Checking a Table's Row Format

The [SHOW TABLE STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-table-status/) statement can be used to see the row format used by a table. For example:

```sql
SHOW TABLE STATUS FROM db1 WHERE Name='tab'\G
*************************** 1. row ***************************
           Name: tab
         Engine: InnoDB
        Version: 10
     Row_format: Dynamic
           Rows: 0
 Avg_row_length: 0
    Data_length: 16384
Max_data_length: 0
   Index_length: 0
      Data_free: 0
 Auto_increment: NULL
    Create_time: 2019-04-18 20:24:04
    Update_time: NULL
     Check_time: NULL
      Collation: latin1_swedish_ci
       Checksum: NULL
 Create_options: row_format=DYNAMIC
        Comment:
```

The [information_schema.INNODB_SYS_TABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-innodb-tables/information-schema-innodb_sys_tables-table/) table can also be queried to see the row format used by a table. For example:

```sql
SELECT * FROM information_schema.INNODB_SYS_TABLES WHERE name='db1/tab'\G
*************************** 1. row ***************************
     TABLE_ID: 42
         NAME: db1/tab
         FLAG: 33
       N_COLS: 4
        SPACE: 27
  FILE_FORMAT: Barracuda
   ROW_FORMAT: Dynamic
ZIP_PAGE_SIZE: 0
   SPACE_TYPE: Single
```

A table's tablespace is tagged with the lowest InnoDB file format that supports the table's row format. So, even if the `Barracuda` file format is enabled, tables that use the `COMPACT` or `REDUNDANT` row formats will be tagged with the `Antelope` file format in the [information_schema.INNODB_SYS_TABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-innodb-tables/information-schema-innodb_sys_tables-table/) table.

## Row Formats

### `REDUNDANT` Row Format

The `REDUNDANT` row format is the original non-compacted row format.

The `REDUNDANT` row format was the only available row format before MySQL 5.0.3. In that release, this row format was retroactively named the `REDUNDANT` row format. In the same release, the `COMPACT` row format was introduced as the new default row format.

See [InnoDB REDUNDANT Row Format](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-redundant-row-format/) for more information.

### `COMPACT` Row Format

##### MariaDB until [10.2.1](/kb/en/mariadb-1021-release-notes/)

In [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/) and before, the default row format is `COMPACT`.

The `COMPACT` row format is similar to the `REDUNDANT` row format, but it stores data in a more compact manner that requires about 20% less storage.

This row format was originally introduced in MySQL 5.0.3.

See [InnoDB COMPACT Row Format](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-compact-row-format/) for more information.

### `DYNAMIC` Row Format

##### MariaDB starting with [10.2.2](/kb/en/mariadb-1022-release-notes/)

In [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/) and later, the default row format is `DYNAMIC`.

The `DYNAMIC` row format is similar to the `COMPACT` row format, but tables using the `DYNAMIC` row format can store even more data on overflow pages than tables using the `COMPACT` row format. This results in more efficient data storage than tables using the `COMPACT` row format, especially for tables containing columns using the [VARBINARY](/columns-storage-engines-and-plugins/data-types/string-data-types/varbinary/), [VARCHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/varchar/), [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob/) and [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text/) data types. However, InnoDB tables using the `COMPRESSED` row format are more efficient.

See [InnoDB DYNAMIC Row Format](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-dynamic-row-format/) for more information.

### `COMPRESSED` Row Format

##### MariaDB starting with [10.1](/kb/en/what-is-mariadb-101/)

In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and later, an alternative way to compress InnoDB tables is by using [InnoDB Page Compression](/kb/en/innodbxtradb-page-compression/).

The `COMPRESSED` row format is similar to the `COMPACT` row format, but tables using the `COMPRESSED` row format can store even more data on overflow pages than tables using the `COMPACT` row format. This results in more efficient data storage than tables using the `COMPACT` row format, especially for tables containing columns using the [VARBINARY](/columns-storage-engines-and-plugins/data-types/string-data-types/varbinary/), [VARCHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/varchar/), [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob/) and [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text/) data types.

The `COMPRESSED` row format also supports compression of all data and index pages.

See [InnoDB COMPRESSED Row Format](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-compressed-row-format/) for more information.

## Maximum Row Size

Several factors help determine the maximum row size of an InnoDB table.

First, MariaDB enforces a 65,535 byte limit on a table's maximum row size. The total size of a table's [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob/) and [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text/) columns do not count towards this limit. Only the pointers for a table's [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob/) and [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text/) columns count towards this limit. MariaDB enforces this limit for all storage engines, so this limit also applies to InnoDB tables. Therefore, this limit is the absolute maximum row size for an InnoDB table.

If you try to create a table that exceeds MariaDB's global limit on a table's maximum row size, then you will see an error like this:

```sql
ERROR 1118 (42000): Row size too large. The maximum row size for the used table type, 
not counting BLOBs, is 65535. This includes storage overhead, check the manual. You 
have to change some columns to TEXT or BLOBs
```

However, InnoDB also has its own limits on the maximum row size, so an InnoDB table's maximum row size could be smaller than MariaDB's global limit.

Second, the maximum amount of data that an InnoDB table can store in a row's main data page depends on the value of the [innodb_page_size](/kb/en/innodb-system-variables/#innodb_page_size) system variable. At most, the data that a single row can consume on the row's main data page is half of the value of the [innodb_page_size](/kb/en/innodb-system-variables/#innodb_page_size) system variable. With the default value of `16k`, that would mean that a single row can consume at most around 8 KB on the row's main data page. However, the limit on the row's main data page is not the absolute limit on the row's size.

Third, all InnoDB row formats can store certain kinds of data in overflow pages, so the maximum row size of an InnoDB table can be larger than the maximum amount of data that can be stored in the row's main data page.

Some row formats can store more data in overflow pages than others. For example, the `DYNAMIC` and `COMPRESSED` row formats can store the most data in overflow pages. To see how to determine the how the various InnoDB row formats can use overflow pages, see the following sections:

- [InnoDB REDUNDANT Row Format: Overflow Pages with the REDUNDANT Row Format](/kb/en/innodb-redundant-row-format/#overflow-pages-with-the-redundant-row-format)
- [InnoDB COMPACT Row Format: Overflow Pages with the COMPACT Row Format](/kb/en/innodb-compact-row-format/#overflow-pages-with-the-compact-row-format)
- [InnoDB DYNAMIC Row Format: Overflow Pages with the DYNAMIC Row Format](/kb/en/innodb-dynamic-row-format/#overflow-pages-with-the-dynamic-row-format)
- [InnoDB COMPRESSED Row Format: Overflow Pages with the COMPRESSED Row Format](/kb/en/innodb-compressed-row-format/#overflow-pages-with-the-compressed-row-format)

If a table's definition can allow rows that the table's InnoDB row format can't actually store, then InnoDB will raise errors or warnings in certain scenarios.

If the table were using the `REDUNDANT` or `COMPACT` row formats, then the error or warning would be the following:

```sql
ERROR 1118 (42000): Row size too large (> 8126). Changing some columns to 
TEXT or BLOB or using ROW_FORMAT=DYNAMIC or ROW_FORMAT=COMPRESSED 
may help. In current row format, BLOB prefix of 768 bytes is stored inline.
```

And if the table were using the `DYNAMIC` or `COMPRESSED` row formats, then the error or warning would be the following:

```sql
ERROR 1118 (42000): Row size too large (> 8126). Changing some columns to 
TEXT or BLOB may help. In current row format, BLOB prefix of 0 bytes is stored inline.
```

These messages are raised in the following cases:

- If [InnoDB strict mode](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-strict-mode/) is <strong>enabled</strong> and if a [DDL](/sql-statements-structure/sql-statements/data-definition/) statement is executed that touches the table, such as [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/) or [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/), then InnoDB will raise an <strong>error</strong> with this message
- If [InnoDB strict mode](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-strict-mode/) is <strong>disabled</strong> and if a [DDL](/sql-statements-structure/sql-statements/data-definition/) statement is executed that touches the table, such as [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/) or [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/), then InnoDB will raise a <strong>warning</strong> with this message.
- Regardless of whether [InnoDB strict mode](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-strict-mode/)  is enabled, if a [DML](/sql-statements-structure/sql-statements/data-manipulation/) statement is executed that attempts to write a row that the table's InnoDB row format can't store, then InnoDB will raise an <strong>error</strong> with this message.

For information on how to solve the problem, see [Troubleshooting Row Size Too Large Errors with InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/troubleshooting-row-size-too-large-errors-with-innodb/).

## Known Issues

### Upgrading Causes Row Size Too Large Errors

Prior to [MariaDB 10.2.26](/kb/en/mariadb-10226-release-notes/), [MariaDB 10.3.17](/kb/en/mariadb-10317-release-notes/), and [MariaDB 10.4.7](/kb/en/mariadb-1047-release-notes/), MariaDB doesn't properly calculate the row sizes while executing DDL. In these versions, <em>unsafe</em> tables can be created, even if [InnoDB strict mode](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-strict-mode/) is enabled. The calculations were fixed by [MDEV-19292](https://jira.mariadb.org/browse/MDEV-19292) in [MariaDB 10.2.26](/kb/en/mariadb-10226-release-notes/), [MariaDB 10.3.17](/kb/en/mariadb-10317-release-notes/), and [MariaDB 10.4.7](/kb/en/mariadb-1047-release-notes/).

As a side effect, some tables that could be created or altered in previous versions may get rejected with the following error in these releases and any later releases.

```sql
ERROR 1118 (42000): Row size too large (> 8126). Changing some columns to 
TEXT or BLOB may help. In current row format, BLOB prefix of 0 bytes is stored inline.
```

And users could also see the following message as an error or warning in the [error log](/mariadb-administration/server-monitoring-logs/error-log/):

```sql
[Warning] InnoDB: Cannot add field col in table db1.tab because after adding it, the row size is 8478 which is greater than maximum allowed size (8126) for a record on index leaf page.
```

InnoDB used the wrong calculations to determine row sizes for quite a long time, so a lot of users may unknowingly have <em>unsafe</em> tables that the InnoDB row format can't actually store.

InnoDB does not currently have an easy way to check which existing tables have this problem. See [MDEV-20400](https://jira.mariadb.org/browse/MDEV-20400) for more information.

For information on how to solve the problem, see [Troubleshooting Row Size Too Large Errors with InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/troubleshooting-row-size-too-large-errors-with-innodb/).