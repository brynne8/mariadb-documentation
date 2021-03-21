# Information Schema TEMP_TABLES_INFO Table

##### MariaDB [10.2.2](/kb/en/mariadb-1022-release-notes/) - [10.2.3](/kb/en/mariadb-1023-release-notes/)

The `TEMP_TABLES_INFO` table was introduced in [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/) and was removed in [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/). See [MDEV-12459](https://jira.mariadb.org/browse/MDEV-12459) progress on an alternative.

The [Information Schema](/kb/en/information_schema/) `TEMP_TABLES_INFO` table contains information about active InnoDB temporary tables. All user and system-created temporary tables are reported when querying this table, with the exception of optimized internal temporary tables. The data is stored in memory.

Previously, InnoDB temp table metadata was rather stored in InnoDB system tables.

It has the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>TABLE_ID</code></td><td>Table ID.</td></tr>
<tr><td><code>NAME</code></td><td>Table name.</td></tr>
<tr><td><code>N_COLS</code></td><td>Number of columns in the temporary table, including three hidden columns that InnoDB creates (<code>DB_ROW_ID</code>, <code>DB_TRX_ID</code>, and <code>DB_ROLL_PTR</code>).</td></tr>
<tr><td><code>SPACE</code></td><td>Numerical identifier for the tablespace identifier holding the temporary table. Compressed temporary tables are stored by default in separate per-table tablespaces in the temporary file directory. For non-compressed tables, the shared temporary table is named <code>ibtmp1</code>, found in the data directory. Always a non-zero value, and regenerated on server restart.</td></tr>
<tr><td><code>PER_TABLE_TABLESPACE</code></td><td>If <code>TRUE</code>, the temporary table resides in a separate per-table tablespace. If <code>FALSE</code>, it resides in the shared temporary tablespace.</td></tr>
<tr><td><code>IS_COMPRESSED</code></td><td><code>TRUE</code> if the table is compressed.</td></tr>
</tbody></table>

The `PROCESS` [privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) is required to view the table.

## Examples

```sql
CREATE TEMPORARY TABLE t (i INT) ENGINE=INNODB;

SELECT * FROM INFORMATION_SCHEMA.INNODB_TEMP_TABLE_INFO;
+----------+--------------+--------+-------+----------------------+---------------+
| TABLE_ID | NAME         | N_COLS | SPACE | PER_TABLE_TABLESPACE | IS_COMPRESSED |
+----------+--------------+--------+-------+----------------------+---------------+
|       39 | #sql1c93_3_1 |      4 |    64 | FALSE                | FALSE         |
+----------+--------------+--------+-------+----------------------+---------------+
```

Adding a compressed table:

```sql
SET GLOBAL innodb_file_format="Barracuda";

CREATE TEMPORARY TABLE t2 (i INT) ROW_FORMAT=COMPRESSED ENGINE=INNODB;

SELECT * FROM INFORMATION_SCHEMA.INNODB_TEMP_TABLE_INFO;
+----------+--------------+--------+-------+----------------------+---------------+
| TABLE_ID | NAME         | N_COLS | SPACE | PER_TABLE_TABLESPACE | IS_COMPRESSED |
+----------+--------------+--------+-------+----------------------+---------------+
|       40 | #sql1c93_3_3 |      4 |    65 | TRUE                 | TRUE          |
|       39 | #sql1c93_3_1 |      4 |    64 | FALSE                | FALSE         |
+----------+--------------+--------+-------+----------------------+---------------+
```