# OPTIMIZE TABLE

## Syntax

```sql
OPTIMIZE [NO_WRITE_TO_BINLOG | LOCAL] TABLE
    tbl_name [, tbl_name] ...
    [WAIT n | NOWAIT]
```

## Description

<code class="fixed" style="white-space:pre-wrap">OPTIMIZE TABLE</code> has two main functions. It can either be used to defragment tables, or to update the InnoDB fulltext index.

##### MariaDB starting with [10.3.0](/kb/en/mariadb-1030-release-notes/)

#### WAIT/NOWAIT

Set the lock wait timeout. See [WAIT and NOWAIT](/sql-statements-structure/sql-statements/transactions/wait-and-nowait/).

### Defragmenting

`OPTIMIZE TABLE` works for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) (before [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/), only if the <a undefined>innodb_file_per_table</a> server system variable is set), [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/), [MyISAM](/kb/en/myisam/) and [ARCHIVE](/columns-storage-engines-and-plugins/storage-engines/archive/) tables, and should be used if you have deleted a large part of a table or if you have made many changes to a table with variable-length
rows (tables that have [VARCHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/varchar/), [VARBINARY](/columns-storage-engines-and-plugins/data-types/string-data-types/varbinary/), [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob/), or [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text/) columns).  Deleted rows are maintained in a
linked list and subsequent <code class="fixed" style="white-space:pre-wrap">INSERT</code> operations reuse old row positions.

This statement requires [SELECT and INSERT privileges](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) for the table.

By default, `OPTIMIZE TABLE` statements are written to the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) and will be [replicated](/replication/). The `NO_WRITE_TO_BINLOG` keyword (`LOCAL` is an alias) will ensure the statement is not written to the binary log.

<code class="fixed" style="white-space:pre-wrap">OPTIMIZE TABLE</code> is also supported for partitioned tables. You
can use 
<code class="highlight fixed" style="white-space:pre-wrap">[ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/) ... OPTIMIZE PARTITION</code> 
to optimize one or more partitions.

You can use <code class="fixed" style="white-space:pre-wrap">OPTIMIZE TABLE</code> to reclaim the unused
space and to defragment the data file. With other storage engines, `OPTIMIZE TABLE` does nothing by default, and returns this message: " The storage engine for the table doesn't support optimize". However, if the server has been started with the `--skip-new` option, `OPTIMIZE TABLE` is linked to [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/), and recreates the table. This operation frees the unused space and updates index statistics.

Since [MariaDB 5.3](/kb/en/what-is-mariadb-53/), the [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/) storage engine supports [progress reporting](/kb/en/progress-reporting/) for this statement.

If a [MyISAM](/kb/en/myisam/) table is fragmented, [concurrent inserts](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/concurrent-inserts/) will not be performed until an `OPTIMIZE TABLE` statement is executed on that table, unless the [concurrent_insert](/kb/en/server-system-variables/#concurrent_insert) server system variable is set to `ALWAYS`.

### Updating an InnoDB fulltext index

When rows are added or deleted to an InnoDB [fulltext index](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes/), the index is not immediately re-organized, as this can be an expensive operation. Change statistics are stored in a separate location . The  fulltext index is only fully re-organized when an `OPTIMIZE TABLE` statement is run.

By default, an OPTIMIZE TABLE will defragment a table. In order to use it to update fulltext index statistics, the [innodb_optimize_fulltext_only](/kb/en/xtradbinnodb-server-system-variables/#innodb_optimize_fulltext_only) system variable must be set to `1`. This is intended to be a temporary setting, and should be reset to `0` once the fulltext index has been re-organized.

Since fulltext re-organization can take a long time, the [innodb_ft_num_word_optimize](/kb/en/xtradbinnodb-server-system-variables/#innodb_ft_num_word_optimize) variable limits the re-organization to a number of words (2000 by default).  You can run multiple OPTIMIZE statements to fully re-organize the index.

### Defragmenting InnoDB tablespaces

##### MariaDB starting with [10.1.1](/kb/en/mariadb-1011-release-notes/)

[MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/) merged the Facebook/Kakao defragmentation patch

[MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/) merged the Facebook/Kakao defragmentation patch, allowing one to use `OPTIMIZE TABLE` to defragment InnoDB tablespaces. For this functionality to be enabled, the [innodb_defragment](/kb/en/xtradbinnodb-server-system-variables/#innodb_defragment) system variable must be enabled. No new tables are created and there is no need to copy data from old tables to new tables. Instead, this feature loads `n` pages (determined by [innodb-defragment-n-pages](/kb/en/xtradbinnodb-server-system-variables/#innodb_defragment_n_pages)) and tries to move records so that pages would be full of records and then frees pages that are fully empty after the operation. Note that tablespace files (including ibdata1) will not shrink as the result of defragmentation, but one will get better memory utilization in the InnoDB buffer pool as there are fewer data pages in use.

See [Defragmenting InnoDB Tablespaces](/replication/optimization-and-tuning/optimizing-tables/defragmenting-innodb-tablespaces/) for more details.

## See Also

[Optimize table in InnoDB](/kb/en/online-ddl-overview/#optimizing-a-table)