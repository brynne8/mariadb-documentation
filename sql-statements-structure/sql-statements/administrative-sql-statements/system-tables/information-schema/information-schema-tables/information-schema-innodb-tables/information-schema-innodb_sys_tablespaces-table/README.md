# Information Schema INNODB_SYS_TABLESPACES Table

##### MariaDB starting with [10.0.4](/kb/en/mariadb-1004-release-notes/)

The `INNODB_SYS_TABLESPACES` table was added in [MariaDB 10.0.4](/kb/en/mariadb-1004-release-notes/).

The [Information Schema](/kb/en/information_schema/) `INNODB_SYS_TABLESPACES` table contains information about InnoDB tablespaces. Until [MariaDB 10.5](/kb/en/what-is-mariadb-105/) it was based on the internal `SYS_TABLESPACES` table. This internal table was removed in [MariaDB 10.6.0](/kb/en/mariadb-1060-release-notes/), so this Information Schema table has been repurposed
to directly reflect the filesystem (fil_system.space_list).

The `PROCESS` [privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) is required to view the table.

It has the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>SPACE</code></td><td>Unique InnoDB tablespace identifier.</td></tr>
<tr><td><code>NAME</code></td><td>Database and table name separated by a backslash, or the uppercase InnoDB system table name.</td></tr>
<tr><td><code>FLAG</code></td><td><code>1</code> if a <code>DATA DIRECTORY</code> option has been specified in <code><a href="/kb/en/create-table/">CREATE TABLE</a></code>, otherwise <code>0</code>.</td></tr>
<tr><td><code>FILE_FORMAT</code></td><td><a href="/kb/en/xtradbinnodb-file-format/">InnoDB file format</a>.</td></tr>
<tr><td><code>ROW_FORMAT</code></td><td><a href="/kb/en/xtradbinnodb-storage-formats/">InnoDB storage format</a> used for this tablespace. If the <a href="/kb/en/xtradbinnodb-file-format/#antelope">Antelope</a> file format is used, this value is always <code>Compact or Redundant</code>.</td></tr>
<tr><td><code>PAGE_SIZE</code></td><td>Page size in bytes for this tablespace. Until <a href="/kb/en/mariadb-1050-release-notes/">MariaDB 10.5.0</a>, this was the value of the <a href="/kb/en/innodb-system-variables/#innodb_page_size">innodb_page_size</a> variable. From <a href="/kb/en/mariadb-1060-release-notes/">MariaDB 10.6.0</a>, contains the physical page size of a page (previously <code>ZIP_PAGE_SIZE</code>).</td></tr>
<tr><td><code>ZIP_PAGE_SIZE</code></td><td>Zip page size for this tablespace. Removed in <a href="/kb/en/mariadb-1060-release-notes/">MariaDB 10.6.0</a>.</td></tr>
<tr><td><code>SPACE_TYPE</code></td><td>Tablespace type. Can be <code>General</code> for general tablespaces or <code>Single</code> for file-per-table tablespaces. Introduced <a href="/kb/en/mariadb-1021-release-notes/">MariaDB 10.2.1</a>. Removed <a href="/kb/en/mariadb-1050-release-notes/">MariaDB 10.5.0</a>.</td></tr>
<tr><td><code>FS_BLOCK_SIZE</code></td><td>File system block size. Introduced <a href="/kb/en/mariadb-1021-release-notes/">MariaDB 10.2.1</a>.</td></tr>
<tr><td><code>FILE_SIZE</code></td><td>Maximum size of the file, uncompressed. Introduced <a href="/kb/en/mariadb-1021-release-notes/">MariaDB 10.2.1</a>.</td></tr>
<tr><td><code>ALLOCATED_SIZE</code></td><td>Actual size of the file as per space allocated on disk. Introduced <a href="/kb/en/mariadb-1021-release-notes/">MariaDB 10.2.1</a>.</td></tr>
<tr><td><code>FILENAME</code></td><td>Tablespace datafile path, previously part of the <a href="/kb/en/information-schema-innodb_sys_datafiles-table/">INNODB_SYS_DATAFILES table</a>. Added in <a href="/kb/en/mariadb-1060-release-notes/">MariaDB 10.6.0</a>.</td></tr>
</tbody></table>

## Examples

[MariaDB 10.4](/kb/en/what-is-mariadb-104/):

```sql
DESC information_schema.innodb_sys_tablespaces;
+----------------+---------------------+------+-----+---------+-------+
| Field          | Type                | Null | Key | Default | Extra |
+----------------+---------------------+------+-----+---------+-------+
| SPACE          | int(11) unsigned    | NO   |     | 0       |       |
| NAME           | varchar(655)        | NO   |     |         |       |
| FLAG           | int(11) unsigned    | NO   |     | 0       |       |
| FILE_FORMAT    | varchar(10)         | YES  |     | NULL    |       |
| ROW_FORMAT     | varchar(22)         | YES  |     | NULL    |       |
| PAGE_SIZE      | int(11) unsigned    | NO   |     | 0       |       |
| ZIP_PAGE_SIZE  | int(11) unsigned    | NO   |     | 0       |       |
| SPACE_TYPE     | varchar(10)         | YES  |     | NULL    |       |
| FS_BLOCK_SIZE  | int(11) unsigned    | NO   |     | 0       |       |
| FILE_SIZE      | bigint(21) unsigned | NO   |     | 0       |       |
| ALLOCATED_SIZE | bigint(21) unsigned | NO   |     | 0       |       |
+----------------+---------------------+------+-----+---------+-------+
```

From [MariaDB 10.4](/kb/en/what-is-mariadb-104/):

```sql
SELECT * FROM information_schema.INNODB_SYS_TABLESPACES LIMIT 2\G
*************************** 1. row ***************************
         SPACE: 2
          NAME: mysql/innodb_table_stats
          FLAG: 33
   FILE_FORMAT: Barracuda
    ROW_FORMAT: Dynamic
     PAGE_SIZE: 16384
 ZIP_PAGE_SIZE: 0
    SPACE_TYPE: Single
 FS_BLOCK_SIZE: 4096
     FILE_SIZE: 98304
ALLOCATED_SIZE: 98304
*************************** 2. row ***************************
         SPACE: 3
          NAME: mysql/innodb_index_stats
          FLAG: 33
   FILE_FORMAT: Barracuda
    ROW_FORMAT: Dynamic
     PAGE_SIZE: 16384
 ZIP_PAGE_SIZE: 0
    SPACE_TYPE: Single
 FS_BLOCK_SIZE: 4096
     FILE_SIZE: 98304
ALLOCATED_SIZE: 98304
```