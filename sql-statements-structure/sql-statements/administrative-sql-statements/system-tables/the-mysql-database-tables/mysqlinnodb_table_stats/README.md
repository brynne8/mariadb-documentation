# mysql.innodb_table_stats

The `mysql.innodb_table_stats` table stores data related to [InnoDB Persistent Statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/innodb-persistent-statistics/), and contains one row per table.

This table, along with the related [mysql.innodb_index_stats](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlinnodb_index_stats/) table, can be manually updated in order to force or test differing query optimization plans. After updating, `FLUSH TABLE innodb_table_stats` is required to load the changes.

`mysql.innodb_table_stats` is not replicated, although any [ANALYZE TABLE](/sql-statements-structure/sql-statements/table-statements/analyze-table/) statements on the table will be by default..

It contains the following fields:

<table><tbody><tr><th>Field</th><th>Type</th><th>Null</th><th>Key</th><th>Default</th><th>Description</th></tr>
<tr><td><code>database_name</code></td><td><code>varchar(64)</code></td><td>NO</td><td>PRI</td><td><code>NULL</code></td><td>Database name.</td></tr>
<tr><td><code>table_name	</code></td><td><code>varchar(64)</code></td><td>NO</td><td>PRI</td><td><code>NULL</code></td><td>Table, partition or subpartition name.</td></tr>
<tr><td><code>last_update</code></td><td><code>timestamp</code></td><td>NO</td><td></td><td><code>current_timestamp()</code></td><td>Time that this row was last updated.</td></tr>
<tr><td><code>n_rows</code></td><td><code>bigint(20) unsigned</code></td><td>NO</td><td></td><td><code>NULL</code></td><td>Number of rows in the table.</td></tr>
<tr><td><code>clustered_index_size</code></td><td><code>bigint(20) unsigned</code></td><td>NO</td><td></td><td><code>NULL</code></td><td>Size, in pages, of the primary index.</td></tr>
<tr><td><code>sum_of_other_index_sizes</code></td><td><code>bigint(20) unsigned</code></td><td>NO</td><td></td><td><code>NULL</code></td><td>Size, in pages, of non-primary indexes.</td></tr>
</tbody></table>

## Example

```sql
SELECT * FROM mysql.innodb_table_stats\G
*************************** 1. row ***************************
           database_name: mysql
              table_name: gtid_slave_pos
             last_update: 2017-08-19 20:38:34
                  n_rows: 0
    clustered_index_size: 1
sum_of_other_index_sizes: 0
*************************** 2. row ***************************
           database_name: test
              table_name: ft
             last_update: 2017-09-15 12:58:39
                  n_rows: 0
    clustered_index_size: 1
sum_of_other_index_sizes: 2
...
```

## See Also

- [InnoDB Persistent Statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/innodb-persistent-statistics/)
- [mysql.innodb_index_stats](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlinnodb_index_stats/)
- [ANALYZE TABLE](/sql-statements-structure/sql-statements/table-statements/analyze-table/)