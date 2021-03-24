# Information Schema INNODB_SYS_FIELDS Table

The [Information Schema](/kb/en/information_schema/) `INNODB_SYS_FIELDS` table contains information about fields that are part of an InnoDB index.

The `PROCESS` [privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) is required to view the table.

It has the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>INDEX_ID</code></td><td>Index identifier, matching the value from <a href="/kb/en/information-schema-innodb_sys_indexes-table/">INNODB_SYS_INDEXES.INDEX_ID</a>.</td></tr>
<tr><td><code>NAME</code></td><td>Field name, matching the value from <a href="/kb/en/information-schema-innodb_sys_columns-table/">INNODB_SYS_COLUMNS.NAME</a>.</td></tr>
<tr><td><code>POS</code></td><td>Ordinal position of the field within the index, starting from <code>0</code>. This is adjusted as columns are removed.</td></tr>
</tbody></table>

## Example

```sql
SELECT * FROM information_schema.INNODB_SYS_FIELDS LIMIT 3\G
*************************** 1. row ***************************
INDEX_ID: 11
    NAME: ID
     POS: 0
*************************** 2. row ***************************
INDEX_ID: 12
    NAME: FOR_NAME 
     POS: 0
*************************** 3. row ***************************
INDEX_ID: 13
    NAME: REF_NAME 
     POS: 0
3 rows in set (0.00 sec)
```