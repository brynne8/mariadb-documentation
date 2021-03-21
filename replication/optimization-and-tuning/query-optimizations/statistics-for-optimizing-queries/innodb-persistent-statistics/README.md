# InnoDB Persistent Statistics

##### MariaDB starting with [10.0.4](/kb/en/mariadb-1004-release-notes/)

Persistent statistics for InnoDB were introduced in [MariaDB 10.0.4](/kb/en/mariadb-1004-release-notes/).

Before [MariaDB 10.0](/kb/en/what-is-mariadb-100/), InnoDB statistics were not stored on disk, meaning that on server restarts the statistics would need to be recalculated, which is both needless computation, as well as leading to inconsistent query plans.

There are a number of variables that control persistent statistics:

- [innodb_stats_persistent](/kb/en/xtradbinnodb-server-system-variables/#innodb_stats_persistent) - when set (the default) enables InnoDB persistent statistics.
- [innodb_stats_auto_recalc](/kb/en/xtradbinnodb-server-system-variables/#innodb_stats_auto_recalc) - when set (the default), persistent statistics are automatically recalculated when the table changes significantly (more than 10% of the rows)
- [innodb_stats_persistent_sample_pages](/kb/en/xtradbinnodb-server-system-variables/#innodb_stats_persistent_sample_pages) - Number of index pages sampled (default 20) when estimating cardinality and statistics for indexed columns. Increasing this value will increases index statistics accuracy, but use more I/O resources when running [ANALYZE TABLE](/sql-statements-structure/sql-statements/table-statements/analyze-table/).

These settings can be overwritten on a per-table basis by use of the [STATS_PERSISTENT](/kb/en/create-table/#stats_persistent), [STATS_AUTO_RECALC](/kb/en/create-table/#stats_auto_recalc) and [STATS_SAMPLE_PAGES](/kb/en/create-table/#stats_sample_pages) clauses in a [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/) or [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/) statement.

Details of the statistics are stored in two system tables in the [mysql database](the-mysql-database-table):

- [innodb_table_stats](/kb/en/mysqlinnodb_table_stats/)
- [innodb_index_stats](/kb/en/mysqlinnodb_index_stats/)

## See Also

- [Index Statistics](/replication/optimization-and-tuning/optimization-and-indexes/index-statistics/)
- [Engine-independent Statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/engine-independent-table-statistics/)
- [Histogram-based Statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/histogram-based-statistics/)