# Information Schema INNODB_SYS_INDEXES Table

The [Information Schema](/kb/en/information_schema/) `INNODB_SYS_INDEXES` table contains information about InnoDB indexes.

The `PROCESS` [privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) is required to view the table.

It has the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>INDEX_ID</code></td><td>A unique index identifier.</td></tr>
<tr><td><code>NAME</code></td><td>Index name, lowercase for all user-created indexes, or uppercase for implicitly-created indexes; <code>PRIMARY</code> (primary key), <code>GEN_CLUST_INDEX</code> (index representing primary key where there isn't one), <code>ID_IND</code>, <code>FOR_IND</code> (validating foreign key constraint) , <code>REF_IND</code>.</td></tr>
<tr><td><code>TABLE_ID</code></td><td>Table identifier, matching the value from <a href="/kb/en/information-schema-innodb_sys_tables-table/">INNODB_SYS_TABLES.TABLE_ID</a>.</td></tr>
<tr><td><code>TYPE</code></td><td>Numeric type identifier; one of <code>0</code> (secondary index), <code>1</code> (clustered index), <code>2</code> (unique index), <code>3</code> (primary index), <code>32</code> (<a href="/kb/en/full-text-indexes/">full-text index</a>).</td></tr>
<tr><td><code>N_FIELDS</code></td><td>Number of columns in the index. GEN_CLUST_INDEX's have a value of 0 as the index is not based on an actual column in the table.</td></tr>
<tr><td><code>PAGE_NO</code></td><td>Index B-tree's root page number. <code>-1</code> (unused) for full-text indexes, as they are laid out over several auxiliary tables.</td></tr>
<tr><td><code>SPACE</code></td><td>Tablespace identifier where the index resides. <code>0</code> represents the InnoDB system tablespace, while any other value represents a table created in file-per-table mode (see the <a href="/kb/en/innodb-system-variables/#innodb_file_per_table">innodb_file_per_table</a> system variable). Remains unchanged after a <a href="/kb/en/truncate-table/">TRUNCATE TABLE</a> statement, and not necessarily unique.</td></tr>
</tbody></table>

## Example

```sql
SELECT * FROM information_schema.INNODB_SYS_INDEXES LIMIT 3\G
*************************** 1. row ***************************
       INDEX_ID: 11
           NAME: ID_IND
       TABLE_ID: 11
           TYPE: 3
       N_FIELDS: 1
        PAGE_NO: 302
          SPACE: 0
MERGE_THRESHOLD: 50
*************************** 2. row ***************************
       INDEX_ID: 12
           NAME: FOR_IND
       TABLE_ID: 11
           TYPE: 0
       N_FIELDS: 1
        PAGE_NO: 303
          SPACE: 0
MERGE_THRESHOLD: 50
*************************** 3. row ***************************
       INDEX_ID: 13
           NAME: REF_IND
       TABLE_ID: 11
           TYPE: 3
       N_FIELDS: 1
        PAGE_NO: 304
          SPACE: 0
MERGE_THRESHOLD: 50
3 rows in set (0.00 sec)
```