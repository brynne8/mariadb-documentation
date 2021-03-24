# Information Schema INNODB_SYS_FOREIGN Table

The [Information Schema](/kb/en/information_schema/) `INNODB_SYS_FOREIGN` table contains information about InnoDB [foreign keys](/replication/optimization-and-tuning/optimization-and-indexes/foreign-keys/).

The `PROCESS` [privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) is required to view the table.

It has the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>ID</code></td><td>Database name and foreign key name.</td></tr>
<tr><td><code>FOR_NAME</code></td><td>Database and table name of the foreign key child.</td></tr>
<tr><td><code>REF_NAME</code></td><td>Database and table name of the foreign key parent.</td></tr>
<tr><td><code>N_COLS</code></td><td>Number of foreign key index columns.</td></tr>
<tr><td><code>TYPE</code></td><td>Bit flag providing information about the foreign key.</td></tr>
</tbody></table>

The `TYPE` column provides a bit flag with information about the foreign key.  This information is `OR`'ed together to read:

<table><tbody><tr><th>Bit Flag</th><th>Description</th></tr>
<tr><td><code>1</code></td><td><code>ON DELETE CASCADE</code></td></tr>
<tr><td><code>2</code></td><td><code>ON UPDATE SET NULL</code></td></tr>
<tr><td><code>4</code></td><td><code>ON UPDATE CASCADE</code></td></tr>
<tr><td><code>8</code></td><td><code>ON UPDATE SET NULL</code></td></tr>
<tr><td><code>16</code></td><td><code>ON DELETE NO ACTION</code></td></tr>
<tr><td><code>32</code></td><td><code>ON UPDATE NO ACTION</code></td></tr>
</tbody></table>

## Example

```sql
SELECT * FROM INNODB_SYS_FOREIGN\G
*************************** 1. row ***************************
      ID: mysql/innodb_index_stats_ibfk_1
FOR_NAME: mysql/innodb_index_stats
REF_NAME: mysql/innodb_table_stats
  N_COLS: 2
    TYPE: 0
...
```