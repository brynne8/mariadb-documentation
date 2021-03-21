# DDL statements that differ for ColumnStore

In most cases, a ColumnStore table works just as any other MariaDB table. There are however a few differences.

The following table lists the data definition statements (DDL) that differ from normal MariaDB [DDL](/sql-statements-structure/sql-statements/data-definition/) when used on ColumnStore tables.

<table><tbody><tr><th>DDL</th><th>Difference</th></tr>
<tr><td><a href="/kb/en/drop-table/">DROP TABLE</a></td><td>Columnstore supports <a href="/kb/en/columnstore-drop-table/">DROP TABLE ...RESTRICT</a>  which only drops the table in the front end.</td></tr>
<tr><td><a href="/kb/en/rename-table/">RENAME TABLE</a></td><td>ColumnStore doesn't allow one to rename a table between databases.</td></tr>
<tr><td><a href="/kb/en/create-table/">CREATE TABLE</a></td><td>ColumnStore doesn't need indexes, partitions and many other table and column options. See here for <a href="https://mariadb.com/kb/en/mariadb/columnstore-create-table/">ColumnStore Specific Syntax</a></td></tr>
<tr><td><a href="/kb/en/create-index/">CREATE INDEX</a></td><td>ColumnStore doesn't need indexes. Hence an index many not be created on a table that is defined with engine=columnstore</td></tr>
</tbody></table>