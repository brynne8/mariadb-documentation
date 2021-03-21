# Row-based Replication With No Primary Key

The terms <em>master</em> and <em>slave</em> have historically been used in replication, but the terms terms <em>primary</em> and <em>replica</em> are now preferred. The old terms are used throughout the documentation, and in MariaDB commands, although [MariaDB 10.5](/kb/en/what-is-mariadb-105/) has begun the process of renaming. The documentation will follow over time. See [MDEV-18777](https://jira.mariadb.org/browse/MDEV-18777) to follow progress on this effort.

MariaDB improves on row-based [replication](/replication/standard-replication/) (see [binary log formats](/mariadb-administration/server-monitoring-logs/binary-log/binary-log-formats/)) of tables which have no primary key
but do have some other index. This is based in part on the original Percona
patch "row_based_replication_without_primary_key.patch", with some additional
fixes and enhancements.

When row-based replication is used with [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update/) or [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete/),
the slave needs to locate each replicated row based on the value in columns. If
the table contains at least one index, an index lookup will be used (otherwise
a table scan is needed for each row, which is extremely inefficient for all but
the smallest table and generally to be avoided).

In MariaDB, the slave will try to choose a good index among any
available:

- The primary key is used, if there is one.
- Else, the first unique index without NULL-able columns is used, if there is
  one.
- Else, a choice is made among any normal indexes on the table (e.g. a
  [FULLTEXT](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes/) index is not considered).

The choice of which of several non-unique indexes to use is based on the
cardinality of indexes; the one that is most selective (has the smallest
average number of rows per distinct tuple of column values) is preferred. Note
that for this choice to be effective, for most storage engines (like
MyISAM, InnoDB) it is necessary to make sure [ANALYZE TABLE](/sql-statements-structure/sql-statements/table-statements/analyze-table/)
has been run on the slave, otherwise statistics about index cardinality will
not be available. In the absence of index cardinality, the first unique index
will be chosen, if any, else the first non-unique index.

Prior to [MariaDB 5.3](/kb/en/what-is-mariadb-53/), the slave would always choose the first index without
considering cardinality. The slave could even choose an unusable index (like
FULLTEXT) if no other index was available ([MySQL Bug #58997](http://bugs.mysql.com/bug.php?id=58997)), causing row-based
replication to break in this case; this was also fixed in [MariaDB 5.3](/kb/en/what-is-mariadb-53/).

## See Also

- [What is MariaDB 5.3](/kb/en/what-is-mariadb-53/)