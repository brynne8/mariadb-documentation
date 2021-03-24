# Information Schema XTRADB_INTERNAL_HASH_TABLES Table

##### MariaDB starting with [10.0.9](/kb/en/mariadb-1009-release-notes/)

The `XTRADB_INTERNAL_HASH_TABLES` table was added in [MariaDB 10.0.9](/kb/en/mariadb-1009-release-notes/).

The [Information Schema](/kb/en/information_schema/) `XTRADB_INTERNAL_HASH_TABLES` table contains InnoDB/XtraDB hash table memory usage information.

The `PROCESS` [privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) is required to view the table.

It has the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>INTERNAL_HASH_TABLE_NAME</code></td><td>Hash table name</td></tr>
<tr><td><code>TOTAL_MEMORY</code></td><td>Total memory</td></tr>
<tr><td><code>CONSTANT_MEMORY</code></td><td>Constant memory</td></tr>
<tr><td><code>VARIABLE_MEMORY</code></td><td>Variable memory</td></tr>
</tbody></table>

## Example

```sql
SELECT * FROM information_schema.XTRADB_INTERNAL_HASH_TABLES;
+--------------------------------+--------------+-----------------+-----------------+
| INTERNAL_HASH_TABLE_NAME       | TOTAL_MEMORY | CONSTANT_MEMORY | VARIABLE_MEMORY |
+--------------------------------+--------------+-----------------+-----------------+
| Adaptive hash index            |      2217568 |         2213368 |            4200 |
| Page hash (buffer pool 0 only) |       139112 |          139112 |               0 |
| Dictionary Cache               |       613423 |          554768 |           58655 |
| File system                    |       816368 |          812272 |            4096 |
| Lock System                    |       332872 |          332872 |               0 |
| Recovery System                |            0 |               0 |               0 |
+--------------------------------+--------------+-----------------+-----------------+
```