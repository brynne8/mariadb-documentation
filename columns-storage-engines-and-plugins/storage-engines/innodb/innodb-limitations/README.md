# InnoDB Limitations

The [InnoDB storage engine](/columns-storage-engines-and-plugins/storage-engines/innodb/) has the following limitations.

## Limitations on Schema

- InnoDB tables can have a maximum of 1,017 columns.  This includes [virtual generated columns](/sql-statements-structure/sql-statements/data-definition/create/generated-columns/).
- InnoDB tables can have a maximum of 64 secondary indexes.
- A multicolumn index on InnoDB can use a maximum of 16 columns.  If you attempt to create a multicolumn index that uses more than 16 columns, MariaDB returns an Error 1070.

## Limitations on Size

- With the exception of variable-length columns (that is, [VARBINARY](/columns-storage-engines-and-plugins/data-types/string-data-types/varbinary/), [VARCHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/varchar/), [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob/) and [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text/)), rows in InnoDB have a maximum length of roughly half the page size for 4KB, 8KB, 16KB and 32KB page sizes.
- The maximum size for [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob/) and [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text/) columns is 4GB.  This also applies to [LONGBLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/longblob/) and [LONGTEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/longtext/).
- MariaDB imposes a row-size limit of 65,535 bytes for the combined sizes of all columns.  If the table contains [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob/) or [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text/) columns, these only count for 9 - 12 bytes in this calculation, given that their content is stored separately.
- 32-bit operating systems have a maximum file size limit of 2GB.  When working with large tables using this architecture, configure InnoDB to use smaller data files.
- The maximum size for the combined InnoDB log files is 512GB.
- With tablespaces, the minimum size is 10MB, the maximum varies depending on the InnoDB Page Size.

<table><tbody><tr><th>InnoDB Page Size</th><th>Maximum Tablespace Size</th></tr>
<tr><td>4KB</td><td>16TB</td></tr>
<tr><td>8KB</td><td>32TB</td></tr>
<tr><td>16KB</td><td>64TB</td></tr>
<tr><td>32KB</td><td>128TB</td></tr>
<tr><td>64KB</td><td>256TB</td></tr>
</tbody></table>

### Page Sizes

Using the [innodb_page_size](/kb/en/innodb-system-variables/#innodb_page_size) system variable, you can configure the size in bytes for InnoDB pages. Pages default to 16KB. There are certain limitations on how you use this variable.

- MariaDB instances using one page size cannot use data files or log files from an instance using a different page size.
- When using a Page Size of 4KB or 8KB, the maximum index key length is lowered proportionately.

<table><tbody><tr><th>InnoDB Page Size</th><th>Index Key Length</th></tr>
<tr><td>4KB</td><td>768B</td></tr>
<tr><td>8KB</td><td>1536B</td></tr>
<tr><td>16KB</td><td>3072B</td></tr>
</tbody></table>

### Large Prefix Size

Until [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/), the [innodb_large_prefix](/kb/en/innodb-system-variables/#innodb_large_prefix) system variable enabled large prefix sizes. That is, when enabled (the default from [MariaDB 10.2](/kb/en/what-is-mariadb-102/)), InnoDB uses 3072B index key prefixes for `DYNAMIC` and `COMPRESSED` row formats. When disabled, it uses 787B key prefixes for tables of any row format.  Using an index key that exceeds this limit throws an error.

From [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/), InnoDB always use large index key prefixes.

## Limitations on Tables

InnoDB has the following table-specific limitations.

- When you issue a [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete/) statement, InnoDB doesn't regenerate the table, rather it deletes each row from the table one by one.
- When running MariaDB on Windows, InnoDB stores databases and tables in lowercase.  When moving databases and tables in a binary format from Windows to a Unix-like system or from a Unix system to Windows, you need to rename these to use lowercase.
- When using cascading [foreign keys](/replication/optimization-and-tuning/optimization-and-indexes/foreign-keys/), operations in the cascade don't activate triggers.

### Table Analysis

MariaDB supports the use of the [ANALYZE TABLE](/sql-statements-structure/sql-statements/table-statements/analyze-table/) SQL statement to analyze and store table key distribution. When MariaDB executes this statement, it calculates index cardinality through random dives on index trees. This makes it fast, but not always accurate as it does not check all rows. The data is only an estimate and repeated executions of this statement may return different results.

In situations where accurate data from [ANALYZE TABLE](/sql-statements-structure/sql-statements/table-statements/analyze-table/) statements is important, enable the [innodb_stats_persistent](/kb/en/innodb-system-variables/#innodb_stats_persistent) system variable. Additionally, you can use the [innodb_stats_transient_sample_pages](/kb/en/innodb-system-variables/#innodb_stats_transient_sample_pages) system variable to change the number of random dives it performs.

When running [ANALYZE TABLE](/sql-statements-structure/sql-statements/table-statements/analyze-table/) twice on a table in which statements or transactions are running, MariaDB blocks the second [ANALYZE TABLE](/sql-statements-structure/sql-statements/table-statements/analyze-table/) until the statement or transaction is complete.  This occurs because the statement or transaction blocks the second [ANALYZE TABLE](/sql-statements-structure/sql-statements/table-statements/analyze-table/) statement from reloading the table definition, which it must do since the old one was marked as obsolete after the first statement.

### Table Status

Similar to the [ANALYZE TABLE](/sql-statements-structure/sql-statements/table-statements/analyze-table/) statement, [SHOW TABLE STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-table-status/) statements do not provide accurate statistics for InnoDB, except for the physical table size.

The InnoDB storage engine does not maintain internal row counts.  Transactions isolate writes, which means that concurrent transactions will not have the same row counts.

### Auto-incrementing Columns

- When defining an index on an auto-incrementing column, it must be defined in a way that allows the equivalent of `SELECT MAX(col)` lookups on the table.
- Restarting MariaDB may cause InnoDB to reuse old auto-increment values, such as in the case of a transaction that was rolled back.
- When auto-incrementing columns run out of values, [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert/) statements generate duplicate-key errors.

## Transactions and Locks

- You can modify data on a maximum of 96 * 1023 concurrent transactions that generate undo records.
- Of the 128 rollback segments, InnoDB assigns 32 to non-redo logs for transactions that modify temporary tables and related objects, reducing the maximum number of concurrent data-modifying transactions to 96,000, from 128.000.
- The limit is 32,000 concurrent transactions when all data-modifying transactions also modify temporary tables.
- Issuing a [LOCK TABLES](/sql-statements-structure/sql-statements/transactions/lock-tables/) statement sets two locks on each table when the [innodb_table_locks](/kb/en/innodb-system-variables/#innodb_table_locks) system variable is enabled (the default).
- When you commit or roll back a transaction, any locks set in the transaction are released. You don't need to issue [LOCK TABLES](/sql-statements-structure/sql-statements/transactions/lock-tables/) statements when the [autocommit](/kb/en/server-system-variables/#autocommit) variable is enabled, as InnoDB would immediately release the table locks.