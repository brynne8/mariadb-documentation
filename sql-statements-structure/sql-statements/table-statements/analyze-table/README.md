# ANALYZE TABLE

## Syntax

```sql
ANALYZE [NO_WRITE_TO_BINLOG | LOCAL] TABLE tbl_name [,tbl_name ...] 
  [PERSISTENT FOR [ALL|COLUMNS ([col_name [,col_name ...]])] 
    [INDEXES ([index_name [,index_name ...]])]]           
```

## Description

`ANALYZE TABLE` analyzes and stores the key distribution for a
table ([index statistics](/replication/optimization-and-tuning/optimization-and-indexes/index-statistics/)). This statement works with [MyISAM](/kb/en/myisam/), [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/) and [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables. During the analysis, InnoDB will allow reads/writes, and MyISAM/Aria reads/inserts. For MyISAM tables, this statement is equivalent to using [myisamchk --analyze](/clients-utilities/myisam-clients-and-utilities/myisamchk/).

For more information on how the analysis works within InnoDB, see
[InnoDB Limitations](/kb/en/innodb-limitations/#table-analysis).

MariaDB uses the stored key distribution to decide the order in which
tables should be joined when you perform a join on something other than
a constant. In addition, key distributions can be used when deciding
which indexes to use for a specific table within a query.

This statement requires [SELECT and INSERT privileges](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) for the table.

By default, ANALYZE TABLE statements are written to the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) and will be [replicated](/replication/). The `NO_WRITE_TO_BINLOG` keyword (`LOCAL` is an alias) will ensure the statement is not written to the binary log.

`ANALYZE TABLE` is also supported for partitioned tables. You
can use [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/) ... ANALYZE PARTITION to analyze one or
more partitions.

The [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/) storage engine supports [progress reporting](/kb/en/progress-reporting/) for the `ANALYZE TABLE` statement.

## Engine-Independent Statistics

`ANALYZE TABLE` supports [engine-independent statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/engine-independent-table-statistics/). See [Engine-Independent Table Statistics: Collecting Statistics with the ANALYZE TABLE Statement](/kb/en/engine-independent-table-statistics/#collecting-statistics-with-the-analyze-table-statement) for more information.

## See Also

- [Index Statistics](/replication/optimization-and-tuning/optimization-and-indexes/index-statistics/)
- [InnoDB Persistent Statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/innodb-persistent-statistics/)
- [Progress Reporting](/kb/en/progress-reporting/)
- [Engine-independent Statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/engine-independent-table-statistics/)
- [Histogram-based Statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/histogram-based-statistics/)
- [ANALYZE Statement](/sql-statements-structure/sql-statements/administrative-sql-statements/analyze-and-explain-statements/analyze-statement/)