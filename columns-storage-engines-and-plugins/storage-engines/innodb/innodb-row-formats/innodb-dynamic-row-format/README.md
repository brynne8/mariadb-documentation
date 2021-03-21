# InnoDB DYNAMIC Row Format

##### MariaDB starting with [10.2.2](/kb/en/mariadb-1022-release-notes/)

In [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/) and later, the default row format is `DYNAMIC`.

The `DYNAMIC` row format is similar to the `COMPACT` row format, but tables using the `DYNAMIC` row format can store even more data on overflow pages than tables using the `COMPACT` row format. This results in more efficient data storage than tables using the `COMPACT` row format, especially for tables containing columns using the [VARBINARY](/columns-storage-engines-and-plugins/data-types/string-data-types/varbinary/), [VARCHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/varchar/), [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob/) and [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text/) data types. However, InnoDB tables using the `COMPRESSED` row format are more efficient.

The `DYNAMIC` row format was originally introduced in [MariaDB 5.5](/kb/en/what-is-mariadb-55/).

## Using the DYNAMIC Row Format

##### MariaDB starting with [10.2.2](/kb/en/mariadb-1022-release-notes/)

In [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/) and later, the default row format is `DYNAMIC`, as long as the [innodb_default_row_format](/kb/en/innodb-system-variables/#innodb_default_row_format) system variable has not been modified. Therefore, in these versions, the easiest way to create an InnoDB table that uses the `DYNAMIC` row format is by <strong>not</strong> setting the [ROW_FORMAT](/kb/en/create-table/#row_format) table option at all in a [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/) or [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/) statement.

It is recommended to set the [innodb_strict_mode](/kb/en/innodb-system-variables/#innodb_strict_mode) system variable to `ON` when using this row format.

For example:

```sql
SET SESSION innodb_strict_mode=ON;

SET GLOBAL innodb_default_row_format='dynamic';

CREATE TABLE tab (
   id int,
   str varchar(50)
) ENGINE=InnoDB;
```

##### MariaDB until [10.2.1](/kb/en/mariadb-1021-release-notes/)

In [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/) and before, the easiest way to create an InnoDB table that uses the `DYNAMIC` row format is by setting the [ROW_FORMAT](/kb/en/create-table/#row_format) table option to to `DYNAMIC` in a [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/) or [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/) statement.

It is recommended to set the [innodb_strict_mode](/kb/en/innodb-system-variables/#innodb_strict_mode) system variable to `ON` when using this row format.

The `DYNAMIC` row format is only supported by the `Barracuda` [file format](/kb/en/xtradbinnodb-file-format/). As a side effect, in [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and before, the `DYNAMIC` row format is only supported if the [InnoDB file format](/kb/en/xtradbinnodb-file-format/) is `Barracuda`. Therefore, the [innodb_file_format](/kb/en/innodb-system-variables/#innodb_file_format) system variable must be set to `Barracuda` to use these row formats in those versions.

In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and before, the `DYNAMIC` row format is also only supported if the table is in a [file per-table](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-file-per-table-tablespaces/) tablespace. Therefore, the [innodb_file_per_table](/kb/en/innodb-system-variables/#innodb_file_per_table) system variable must be set to `ON` to use this row format in those versions.

For example:

```sql
SET SESSION innodb_strict_mode=ON;

SET GLOBAL innodb_file_per_table=ON;

SET GLOBAL innodb_file_format='Barracuda';

CREATE TABLE tab (
   id int,
   str varchar(50)
) ENGINE=InnoDB ROW_FORMAT=DYNAMIC;
```

## Index Prefixes with the DYNAMIC Row Format

The `DYNAMIC` row format supports index prefixes up to 3072 bytes. In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and before, the [innodb_large_prefix](/kb/en/innodb-system-variables/#innodb_large_prefix) system variable is used to configure the maximum index prefix length. In these versions, if [innodb_large_prefix](/kb/en/innodb-system-variables/#innodb_large_prefix) is set to `ON`, then the maximum prefix length is 3072 bytes, and if it is set to `OFF`, then the maximum prefix length is 767 bytes.

## Overflow Pages with the DYNAMIC Row Format

All InnoDB row formats can store certain kinds of data in overflow pages. This allows for the maximum row size of an InnoDB table to be larger than the maximum amount of data that can be stored in the row's main data page. See [Maximum Row Size](#maximum-row-size) for more information about the other factors that can contribute to the maximum row size for InnoDB tables.

In the `DYNAMIC` row format variable-length columns, such as columns using the [VARBINARY](/columns-storage-engines-and-plugins/data-types/string-data-types/varbinary/), [VARCHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/varchar/), [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob/) and [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text/) data types, can be completely stored in overflow pages.

InnoDB only considers using overflow pages if the table's row size is greater than half of [innodb_page_size](/kb/en/innodb-system-variables/#innodb_page_size). If the row size is greater than this, then InnoDB chooses variable-length columns to be stored on overflow pages until the row size is less than half of [innodb_page_size](/kb/en/innodb-system-variables/#innodb_page_size).

For [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob/) and [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text/) columns, only values longer than 40 bytes are considered for storage on overflow pages. For [VARBINARY](/columns-storage-engines-and-plugins/data-types/string-data-types/varbinary/) and [VARCHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/varchar/) columns, only values longer than 255 bytes are considered for storage on overflow pages. Bytes that are stored to track a value's length do not count towards these limits. These limits are only based on the length of the actual column's data.

These limits differ from the limits for the `COMPACT` row format, where the limit is 767 bytes for all types.

Fixed-length columns greater than 767 bytes are encoded as variable-length columns, so they can also be stored in overflow pages if the table's row size is greater than half of [innodb_page_size](/kb/en/innodb-system-variables/#innodb_page_size). Even though a column using the [CHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/char/) data type can hold at most 255 characters, a [CHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/char/) column can still exceed 767 bytes in some cases. For example, a `char(255)` column can exceed 767 bytes if the [character set](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/) is `utf8mb4`.

If a column is chosen to be stored on overflow pages, then the entire value of the column is stored on overflow pages, and only a 20-byte pointer to the column's first overflow page is stored on the main page. Each overflow page is the size of [innodb_page_size](/kb/en/innodb-system-variables/#innodb_page_size). If a column is too large to be stored on a single overflow page, then it is stored on multiple overflow pages. Each overflow page contains part of the data and a 20-byte pointer to the next overflow page, if a next page exists.

This behavior differs from the behavior of the `COMPACT` row format, which always stores the column prefix on the main page. This allows tables using the `DYNAMIC` row format to contain a high number of columns using the [VARBINARY](/columns-storage-engines-and-plugins/data-types/string-data-types/varbinary/), [VARCHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/varchar/), [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob/) and [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text/) data types.