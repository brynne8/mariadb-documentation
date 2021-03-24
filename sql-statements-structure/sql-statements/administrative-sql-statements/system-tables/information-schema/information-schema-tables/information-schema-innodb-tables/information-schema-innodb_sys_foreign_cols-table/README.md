# Information Schema INNODB_SYS_FOREIGN_COLS Table

The [Information Schema](/kb/en/information_schema/) `INNODB_SYS_FOREIGN_COLS` table contains information about InnoDB [foreign key](/replication/optimization-and-tuning/optimization-and-indexes/foreign-keys/) columns.

The `PROCESS` [privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) is required to view the table.

It has the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>ID</code></td><td>Foreign key index associated with this column, matching the <a href="/kb/en/information-schema-innodb_sys_foreign-table/">INNODB_SYS_FOREIGN.ID</a> field.</td></tr>
<tr><td><code>FOR_COL_NAME</code></td><td>Child column name.</td></tr>
<tr><td><code>REF_COL_NAME</code></td><td>Parent column name.</td></tr>
<tr><td><code>POS</code></td><td>Ordinal position of the column in the table, starting from 0.</td></tr>
</tbody></table>