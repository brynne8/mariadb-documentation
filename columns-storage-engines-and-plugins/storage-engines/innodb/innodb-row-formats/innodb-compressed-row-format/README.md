# InnoDB COMPRESSED Row Format

In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and later, an alternative (and usually superior) way to compress InnoDB tables is by using [InnoDB Page Compression](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-page-compression). See [Comparison with the COMPRESSED Row Format](/kb/en/innodb-page-compression/#comparison-with-the-compressed-row-format).

The `COMPRESSED` row format is similar to the `COMPACT` row format, but tables using the `COMPRESSED` row format can store even more data on overflow pages than tables using the `COMPACT` row format. This results in more efficient data storage than tables using the `COMPACT` row format, especially for tables containing columns using the [VARBINARY](/columns-storage-engines-and-plugins/data-types/string-data-types/varbinary), [VARCHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/varchar), [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob) and [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text) data types.

The `COMPRESSED` row format also supports compression of all data and index pages.

## Using the `COMPRESSED` Row Format

An InnoDB table that uses the `COMPRESSED` row format can be created by setting the [ROW_FORMAT](/kb/en/create-table/#row_format) table option to `COMPRESSED` and by setting the [KEY_BLOCK_SIZE](/kb/en/create-table/#key_block_size) table option to one of the following values in a [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table) or [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table) statement, where the units are in `KB`:

- `1`
- `2`
- `4`
- `8`
- `16`

`16k` is the default value of the [innodb_page_size](/kb/en/innodb-system-variables/#innodb_page_size) system variable, so using `16` will usually result in minimal compression unless one of the following is true:

- The table has many columns that can be stored in overflow pages, such as columns that use the [VARBINARY](/columns-storage-engines-and-plugins/data-types/string-data-types/varbinary), [VARCHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/varchar), [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob) and [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text) data types.
- The server is using a non-default [innodb_page_size](/kb/en/innodb-system-variables/#innodb_page_size) value that is greater than `16k`.

In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and later, the value of the [innodb_page_size](/kb/en/innodb-system-variables/#innodb_page_size) system variable can be set to `32k` and `64k`. This is especially useful because the larger page size permits more columns using the [VARBINARY](/columns-storage-engines-and-plugins/data-types/string-data-types/varbinary), [VARCHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/varchar), [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob) and [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text) data types. Regardless, even when the value of the [innodb_page_size](/kb/en/innodb-system-variables/#innodb_page_size) system variable is set to some value higher than `16k`, `16` is still the maximum value for the [KEY_BLOCK_SIZE](/kb/en/create-table/#key_block_size) table option for InnoDB tables using the `COMPRESSED` row format.

The `COMPRESSED` row format cannot be set as the default row format with the [innodb_default_row_format](/kb/en/innodb-system-variables/#innodb_default_row_format) system variable.

The `COMPRESSED` row format is only supported by the `Barracuda` [file format](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-file-format). As a side effect, in [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and before, the `COMPRESSED` row format is only supported if the [InnoDB file format](/kb/en/xtradbinnodb-file-format/) is `Barracuda`. Therefore, the [innodb_file_format](/kb/en/innodb-system-variables/#innodb_file_format) system variable must be set to `Barracuda` to use these row formats in those versions.

In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and before, the `COMPRESSED` row format is also only supported if the table is in a [file per-table](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-file-per-table-tablespaces) tablespace. Therefore, the [innodb_file_per_table](/kb/en/innodb-system-variables/#innodb_file_per_table) system variable must be set to `ON` to use this row format in those versions.

It is also recommended to set the [innodb_strict_mode](/kb/en/innodb-system-variables/#innodb_strict_mode) system variable to `ON` when using this row format.

InnoDB automatically uses the `COMPRESSED` row format for a table if the [KEY_BLOCK_SIZE](/kb/en/create-table/#key_block_size) table option is set to some value in a [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table) or [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table) statement. For example:

```sql
SET SESSION innodb_strict_mode=ON;

SET GLOBAL innodb_file_per_table=ON;

SET GLOBAL innodb_file_format='Barracuda';

CREATE TABLE tab (
   id int,
   str varchar(50)
) ENGINE=InnoDB KEY_BLOCK_SIZE=4;
```

If the [KEY_BLOCK_SIZE](/kb/en/create-table/#key_block_size) table option is <strong>not</strong> set to some value, but the [ROW_FORMAT](/kb/en/create-table/#row_format) table option is set to `COMPRESSED` in a [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table) or [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table) statement, then InnoDB uses a default value of `8` for the [KEY_BLOCK_SIZE](/kb/en/create-table/#key_block_size) table option. For example:

```sql
SET SESSION innodb_strict_mode=ON;

SET GLOBAL innodb_file_per_table=ON;

SET GLOBAL innodb_file_format='Barracuda';

CREATE TABLE tab (
   id int,
   str varchar(50)
) ENGINE=InnoDB ROW_FORMAT=COMPRESSED;
```

## Compression with the `COMPRESSED` Row Format

The `COMPRESSED` row format supports compression of all data and index pages.

To avoid compressing and uncompressing pages too many times, InnoDB tries to keep both compressed and uncompressed pages in the [buffer pool](/kb/en/xtradbinnodb-memory-buffer/) when there is enough room. This results in a bigger cache. When there is not enough room, an adaptive LRU algorithm is used to decide whether compressed or uncompressed pages should be evicted from the buffer: for CPU-bound workloads, the compressed pages are evicted first; for I/O-bound workloads, the uncompressed pages are evicted first. Of course, when necessary, both the compressed and uncompressed version of the same data can be evicted from the buffer.

Each compressed page has an uncompressed <em>modification log</em>, stored within the page itself. InnoDB writes small changes into it. When the space in the modification log runs out, the page is uncompressed, changes are applied, and the page is recompressed again. This is done to avoid some unnecessary decompression and compression operations.

Sometimes a <em>compression failure</em> might happen, because the data has grown too much to fit the page. When this happens, the page (and the index node) is split into two different pages. This process can be repeated recursively until the data fit the pages. This can be CPU-consuming on some busy servers which perform many write operations.

Before writing a compressed page into a data file, InnoDB writes it into the [redo log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-redo-log). This ensures that the [redo log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-redo-log) can always be used to recover tables after a crash, even if the compression library is updated and some incompatibilities are introduced. But this also means that the [redo log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-redo-log) will grow faster and might need more space, or the frequency of checkpoints might need to increase.

## Monitoring Performance of the `COMPRESSED` Row Format

The following `INFORMATION_SCHEMA` tables can be used to monitor the performances of InnoDB compressed tables:

- [INNODB_CMP and INNODB_CMP_RESET](/kb/en/information_schemainnodb_cmp-and-innodb_cmp_reset-tables/)
- [INNODB_CMP_PER_INDEX and INNODB_CMP_PER_INDEX_RESET](/kb/en/information_schemainnodb_cmp_per_index-and-innodb_cmp_per_index_reset-table/)
- [INNODB_CMPMEM and INNODB_CMPMEM_RESET](/kb/en/information_schemainnodb_cmpmem-and-innodb_cmpmem_reset-tables/)

## Index Prefixes with the `COMPRESSED` Row Format

The `COMPRESSED` row format supports index prefixes up to 3072 bytes. In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and before, the [innodb_large_prefix](/kb/en/innodb-system-variables/#innodb_large_prefix) system variable is used to configure the maximum index prefix length. In these versions, if [innodb_large_prefix](/kb/en/innodb-system-variables/#innodb_large_prefix) is set to `ON`, then the maximum prefix length is 3072 bytes, and if it is set to `OFF`, then the maximum prefix length is 767 bytes.

## Overflow Pages with the `COMPRESSED` Row Format

All InnoDB row formats can store certain kinds of data in overflow pages. This allows for the maximum row size of an InnoDB table to be larger than the maximum amount of data that can be stored in the row's main data page. See [Maximum Row Size](#maximum-row-size) for more information about the other factors that can contribute to the maximum row size for InnoDB tables.

In the `COMPRESSED` row format variable-length columns, such as columns using the [VARBINARY](/columns-storage-engines-and-plugins/data-types/string-data-types/varbinary), [VARCHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/varchar), [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob) and [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text) data types, can be completely stored in overflow pages.

InnoDB only considers using overflow pages if the table's row size is greater than half of [innodb_page_size](/kb/en/innodb-system-variables/#innodb_page_size). If the row size is greater than this, then InnoDB chooses variable-length columns to be stored on overflow pages until the row size is less than half of [innodb_page_size](/kb/en/innodb-system-variables/#innodb_page_size).

For [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob) and [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text) columns, only values longer than 40 bytes are considered for storage on overflow pages. For [VARBINARY](/columns-storage-engines-and-plugins/data-types/string-data-types/varbinary) and [VARCHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/varchar) columns, only values longer than 255 bytes are considered for storage on overflow pages. Bytes that are stored to track a value's length do not count towards these limits. These limits are only based on the length of the actual column's data.

These limits differ from the limits for the `COMPACT` row format, where the limit is 767 bytes for all types.

Fixed-length columns greater than 767 bytes are encoded as variable-length columns, so they can also be stored in overflow pages if the table's row size is greater than half of [innodb_page_size](/kb/en/innodb-system-variables/#innodb_page_size). Even though a column using the [CHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/char) data type can hold at most 255 characters, a [CHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/char) column can still exceed 767 bytes in some cases. For example, a `char(255)` column can exceed 767 bytes if the [character set](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets) is `utf8mb4`.

If a column is chosen to be stored on overflow pages, then the entire value of the column is stored on overflow pages, and only a 20-byte pointer to the column's first overflow page is stored on the main page. Each overflow page is the size of [innodb_page_size](/kb/en/innodb-system-variables/#innodb_page_size). If a column is too large to be stored on a single overflow page, then it is stored on multiple overflow pages. Each overflow page contains part of the data and a 20-byte pointer to the next overflow page, if a next page exists.

This behavior differs from the behavior of the `COMPACT` row format, which always stores the column prefix on the main page. This allows tables using the `COMPRESSED` row format to contain a high number of columns using the [VARBINARY](/columns-storage-engines-and-plugins/data-types/string-data-types/varbinary), [VARCHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/varchar), [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob) and [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text) data types.

## Read-Only

##### MariaDB starting with [10.6](/kb/en/what-is-mariadb-106/)

From [MariaDB 10.6.0](/kb/en/mariadb-1060-release-notes/), tables that are of the `COMPRESSED` row format are read-only by default. This is the first step towards removing write support and deprecating the feature.

Set the  [innodb_read_only_compressed](/kb/en/innodb-system-variables/#innodb_read_only_compressed) variable to `OFF` to make the tables writable.

## See Also

- [InnoDB Page Compression](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-page-compression)
- [Storage-Engine Independent Column Compression](/replication/optimization-and-tuning/optimization-and-tuning-compression/storage-engine-independent-column-compression)