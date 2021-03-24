# Information Schema INNODB_SYS_TABLES Table

The [Information Schema](/kb/en/information_schema/) `INNODB_SYS_TABLES` table contains information about InnoDB tables.

The `PROCESS` [privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) is required to view the table.

It has the following columns:

<table><tbody><tr><th>Column</th><th>Description</th><th>Added</th></tr>
<tr><td><code>TABLE_ID</code></td><td>Unique InnoDB table identifier.</td><td></td></tr>
<tr><td><code>NAME</code></td><td>Database and table name, or the uppercase InnoDB system table name.</td><td></td></tr>
<tr><td><code>FLAG</code></td><td>See <a href="#flag">Flag</a> below.</td><td></td></tr>
<tr><td><code>N_COLS</code></td><td>Number of columns in the table.</td><td></td></tr>
<tr><td><code>SPACE</code></td><td>Tablespace identifier where the index resides. <code>0</code> represents the InnoDB system tablespace, while any other value represents a table created in file-per-table mode (see the <a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_file_per_table">innodb_file_per_table</a> system variable). Remains unchanged after a <code><a href="/kb/en/truncate-table/">TRUNCATE TABLE</a></code> statement.</td><td></td></tr>
<tr><td><code>FILE_FORMAT</code></td><td><a href="/kb/en/xtradbinnodb-file-format/">InnoDB file format</a> (Antelope or Barracuda).</td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><code>ROW_FORMAT</code></td><td><a href="/kb/en/xtradbinnodb-storage-formats/">InnoDB storage format</a> (Compact, Redundant, Dynamic, or Compressed).</td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><code>ZIP_PAGE_SIZE</code></td><td>For Compressed tables, the zipped page size.</td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
</tbody></table>

## Flag

The flag field returns the dict_table_t::flags that correspond to the data dictionary record.

<table><tbody><tr><th>Bit</th><th>Description</th></tr>
<tr><td><code>0</code></td><td>Set if ROW_FORMAT is not REDUNDANT.</td></tr>
<tr><td><code>1</code> to <code>4</code></td><td><code>0</code>, except for ROW_FORMAT=COMPRESSED, where they will determine the KEY_BLOCK_SIZE (the compressed page size).</td></tr>
<tr><td><code>5</code></td><td>Set for ROW_FORMAT=DYNAMIC or ROW_FORMAT=COMPRESSED.</td></tr>
<tr><td><code>6</code></td><td>Set if the DATA DIRECTORY attribute was present when the table was originally created.</td></tr>
<tr><td><code>7</code></td><td>Set if the page_compressed attribute is present.</td></tr>
<tr><td><code>8</code> to <code>11</code></td><td>Determine the page_compression_level.</td></tr>
<tr><td><code>12</code> <code>13</code></td><td>Normally <code>00</code>, but <code>11</code> for "no-rollback tables" (<a href="/kb/en/what-is-mariadb-103/">MariaDB 10.3</a> CREATE SEQUENCE). In <a href="/kb/en/what-is-mariadb-101/">MariaDB 10.1</a>, these bits could be <code>01</code> or <code>10</code> for ATOMIC_WRITES=ON or ATOMIC_WRITES=OFF.</td></tr>
</tbody></table>

Note that the table flags returned here are not the same as tablespace flags (FSP_SPACE_FLAGS).

## Example

```sql
SELECT * FROM information_schema.INNODB_SYS_TABLES LIMIT 2\G
*************************** 1. row ***************************
     TABLE_ID: 14
         NAME: SYS_DATAFILES
         FLAG: 0
       N_COLS: 5
        SPACE: 0
  FILE_FORMAT: Antelope
   ROW_FORMAT: Redundant
ZIP_PAGE_SIZE: 0
   SPACE_TYPE: System
*************************** 2. row ***************************
     TABLE_ID: 11
         NAME: SYS_FOREIGN
         FLAG: 0
       N_COLS: 7
        SPACE: 0
  FILE_FORMAT: Antelope
   ROW_FORMAT: Redundant
ZIP_PAGE_SIZE: 0
   SPACE_TYPE: System
2 rows in set (0.00 sec)
```

## See Also

- [InnoDB Data Dictionary Troubleshooting](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-troubleshooting/innodb-data-dictionary-troubleshooting/)