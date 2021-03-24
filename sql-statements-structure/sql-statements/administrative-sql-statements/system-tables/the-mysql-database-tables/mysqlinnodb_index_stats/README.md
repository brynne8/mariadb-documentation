# mysql.innodb_index_stats

The `mysql.innodb_index_stats` table stores data related to particular [InnoDB Persistent Statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/innodb-persistent-statistics/), and contains multiple rows for each index.

This table, along with the related [mysql.innodb_table_stats](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlinnodb_table_stats/) table, can be manually updated in order to force or test differing query optimization plans. After updating, `FLUSH TABLE innodb_index_stats` is required to load the changes.

`mysql.innodb_index_stats` is not replicated, although any [ANALYZE TABLE](/sql-statements-structure/sql-statements/table-statements/analyze-table/) statements on the table will be by default..

It contains the following fields:

<table><tbody><tr><th>Field</th><th>Type</th><th>Null</th><th>Key</th><th>Default</th><th>Description</th></tr>
<tr><td><code>database_name</code></td><td><code>varchar(64)</code></td><td>NO</td><td>PRI</td><td><code>NULL</code></td><td>Database name.</td></tr>
<tr><td><code>table_name</code></td><td><code>varchar(64)</code></td><td>NO</td><td>PRI</td><td><code>NULL</code></td><td>Table, partition or subpartition name.</td></tr>
<tr><td><code>index_name</code></td><td><code>varchar(64)</code></td><td>NO</td><td>PRI</td><td><code>NULL</code></td><td>Index name.</td></tr>
<tr><td><code>last_update</code></td><td><code>timestamp</code></td><td>NO</td><td></td><td><code>current_timestamp()</code></td><td>Time that this row was last updated.</td></tr>
<tr><td><code>stat_name</code></td><td><code>varchar(64)</code></td><td>NO</td><td>PRI</td><td><code>NULL</code></td><td>Statistic name.</td></tr>
<tr><td><code>stat_value</code></td><td><code>bigint(20) unsigned</code></td><td>NO</td><td></td><td><code>NULL</code></td><td>Estimated statistic value.</td></tr>
<tr><td><code>sample_size</code></td><td><code>bigint(20) unsigned</code></td><td>YES</td><td></td><td><code>NULL</code></td><td>Number of pages sampled for the estimated statistic value.</td></tr>
<tr><td><code>stat_description</code></td><td><code>varchar(1024)</code></td><td>NO</td><td></td><td><code>NULL</code></td><td>Statistic description.</td></tr>
</tbody></table>

## Example

```sql
SELECT * FROM mysql.innodb_index_stats\G
*************************** 1. row ***************************
   database_name: mysql
      table_name: gtid_slave_pos
      index_name: PRIMARY
     last_update: 2017-08-19 20:38:34
       stat_name: n_diff_pfx01
      stat_value: 0
     sample_size: 1
stat_description: domain_id
*************************** 2. row ***************************
   database_name: mysql
      table_name: gtid_slave_pos
      index_name: PRIMARY
     last_update: 2017-08-19 20:38:34
       stat_name: n_diff_pfx02
      stat_value: 0
     sample_size: 1
stat_description: domain_id,sub_id
*************************** 3. row ***************************
   database_name: mysql
      table_name: gtid_slave_pos
      index_name: PRIMARY
     last_update: 2017-08-19 20:38:34
       stat_name: n_leaf_pages
      stat_value: 1
     sample_size: NULL
stat_description: Number of leaf pages in the index
*************************** 4. row ***************************
   database_name: mysql
      table_name: gtid_slave_pos
      index_name: PRIMARY
     last_update: 2017-08-19 20:38:34
       stat_name: size
      stat_value: 1
     sample_size: NULL
stat_description: Number of pages in the index
*************************** 5. row ***************************
   database_name: test
      table_name: ft
      index_name: FTS_DOC_ID_INDEX
     last_update: 2017-09-15 12:58:39
       stat_name: n_diff_pfx01
      stat_value: 0
     sample_size: 1
stat_description: FTS_DOC_ID
*************************** 6. row ***************************
   database_name: test
      table_name: ft
      index_name: FTS_DOC_ID_INDEX
     last_update: 2017-09-15 12:58:39
       stat_name: n_leaf_pages
      stat_value: 1
     sample_size: NULL
stat_description: Number of leaf pages in the index
...
```

## See Also

- [InnoDB Persistent Statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/innodb-persistent-statistics/)
- [mysql.innodb_table_stats](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlinnodb_table_stats/)
- [ANALYZE TABLE](/sql-statements-structure/sql-statements/table-statements/analyze-table/)