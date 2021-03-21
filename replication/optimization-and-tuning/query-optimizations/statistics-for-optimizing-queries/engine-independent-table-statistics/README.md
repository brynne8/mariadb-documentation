# Engine-Independent Table Statistics

##### MariaDB starting with [10.0.1](/kb/en/mariadb-1001-release-notes/)

The engine-independent table statistics feature was first implemented in [MariaDB 10.0.1](/kb/en/mariadb-1001-release-notes/) and, it was first enabled for queries by default in [MariaDB 10.4](/kb/en/what-is-mariadb-104/).

## Introduction

Before [MariaDB 10.0](/kb/en/what-is-mariadb-100/), the MySQL/MariaDB optimizer relied on storage engines (e.g. InnoDB) to provide statistics for the query optimizer. This approach worked; however it had some deficiencies:

- Storage engines provided poor statistics (this was
fixed to some degree with the introduction of [Persistent Statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/innodb-persistent-statistics/)).
- The statistics were supplied through the MySQL Storage Engine Interface, which puts a lot of restrictions on what kind of data is supplied (for example, there is no way to get any data about value distribution in a non-indexed column)
- There was little control of the statistics. There was no way to "pin" current statistic values, or provide some values on your own, etc.

Engine-independent table statistics lift these limitations.

- Statistics are stored in regular tables in the `mysql` database.
<ul start="1"><li>it is possible for a DBA to read and update the values.
</li></ul>
- More data is collected/used.

Statistics are stored in three tables, [mysql.table_stats](/kb/en/mysqltable_stats-table/), [mysql.column_stats](/kb/en/mysqlcolumn_stats-table/) and [mysql.index_stats](/kb/en/mysqlindex_stats-table/).

Use or update of data from these tables is controlled by [use_stat_tables](/kb/en/server-system-variables/#use_stat_tables) variable. Possible values are listed below:

<table><tbody><tr><th>Value</th><th>Meaning</th></tr>
<tr><td>'never'</td><td>The optimizer doesn't use data from statistics tables. Default for <a href="/kb/en/mariadb-1040-release-notes/">MariaDB 10.4.0</a> and below.</td></tr>
<tr><td>'complementary'</td><td>The optimizer uses data from statistics tables if the same kind of data is not provided by the storage engine.</td></tr>
<tr><td>'preferably'</td><td>Prefer the data from statistics tables, if it's not available there, use the data from the storage engine.</td></tr>
<tr><td>'complementary_for_queries'</td><td>Same as <code>complementary</code>, but for queries only (to avoid needlessly collecting for <a href="/kb/en/analyze-table/">ANALYZE TABLE</a>). From <a href="/kb/en/mariadb-1041-release-notes/">MariaDB 10.4.1</a>.</td></tr>
<tr><td>'preferably_for_queries'</td><td>Same as <code>preferably</code>, but for queries only (to avoid needlessly collecting for <a href="/kb/en/analyze-table/">ANALYZE TABLE</a>). Available and default from <a href="/kb/en/mariadb-1041-release-notes/">MariaDB 10.4.1</a>.</td></tr>
</tbody></table>

## Collecting Statistics with the ANALYZE TABLE Statement

The [ANALYZE TABLE](/sql-statements-structure/sql-statements/table-statements/analyze-table/) statement can be used to collect table statistics. For example:

```sql
ANALYZE TABLE table_name;
```

When the [ANALYZE TABLE](/sql-statements-structure/sql-statements/table-statements/analyze-table/) statement is executed, MariaDB makes a call to the table's storage engine, and the storage engine collects its own statistics for the table. The specific behavior depends on the storage engine. For [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/), see [InnoDB Persistent Statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/innodb-persistent-statistics/) for more information.

When the [ANALYZE TABLE](/sql-statements-structure/sql-statements/table-statements/analyze-table/) statement is executed, MariaDB may also collect engine-independent statistics for the table. The specific behavior depends on the value of the <a undefined>use_stat_tables</a> system variable. Engine-independent statistics will only be collected by the [ANALYZE TABLE](/sql-statements-structure/sql-statements/table-statements/analyze-table/) statement if one of the following is true:

- The <a undefined>use_stat_tables</a> system variable is set to `complementary` or `preferably`.
- The [ANALYZE TABLE](/sql-statements-structure/sql-statements/table-statements/analyze-table/) statement includes the `PERSISTENT FOR` clause.

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, the <a undefined>use_stat_tables</a> system variable is set to `preferably_for_queries` by default. With this value, engine-independent statistics are used by default, but they are not collected by default. If you want to use engine-independent statistics with the default configuration, then you will have to collect them by executing the [ANALYZE TABLE](/sql-statements-structure/sql-statements/table-statements/analyze-table/) statement and by specifying the `PERSISTENT FOR` clause. It is recommended to collect engine-independent statistics on as-needed basis, so typically one will not have engine-independent statistics for all indexes/all columns.

Engine-independent statistics are collected by doing full table and full index scans, and this process can be quite expensive.

### Collecting Statistics for Specific Columns or Indexes

The syntax for the [ANALYZE TABLE](/sql-statements-structure/sql-statements/table-statements/analyze-table/) statement has been extended with the `PERSISTENT FOR` clause. This clause allows one to collect engine-independent statistics only for particular columns or indexes. This clause also allows one to collect engine-independent statistics, regardless of the value of the <a undefined>use_stat_tables</a> system variable. For example:

```sql
ANALYZE TABLE table_name PERSISTENT FOR ALL;
```

Statistics for columns using the [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob/) and [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text/) data types are not collected. If a column using one of these types is explicitly specified, then a warning is returned.

### Examples of Statistics Collection

```sql
-- update all engine-independent statistics for all columns and indexes
ANALYZE TABLE tbl PERSISTENT FOR ALL;

-- update specific columns and indexes:
ANALYZE TABLE tbl PERSISTENT FOR COLUMNS (col1,col2,...) INDEXES (idx1,idx2,...);

-- empty lists are allowed:
ANALYZE TABLE tbl PERSISTENT FOR COLUMNS (col1,col2,...) INDEXES ();
ANALYZE TABLE tbl PERSISTENT FOR COLUMNS () INDEXES (idx1,idx2,...);

-- the following will only update mysql.table_stats fields:
ANALYZE TABLE tbl PERSISTENT FOR COLUMNS () INDEXES ();

-- when use_stat_tables is set to 'COMPLEMENTARY' or 'PREFERABLY', 
-- a simple ANALYZE TABLE  collects engine-independent statistics for all columns and indexes.
SET SESSION use_stat_tables='COMPLEMENTARY';
ANALYZE TABLE tbl;
```

## Manual Updates to Statistics Tables

Statistics are stored in three tables, [mysql.table_stats](/kb/en/mysqltable_stats-table/), [mysql.column_stats](/kb/en/mysqlcolumn_stats-table/) and [mysql.index_stats](/kb/en/mysqlindex_stats-table/).

It is possible to update statistics tables manually. One should modify the table(s) with regular [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert/)/[UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update/)/[DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete/) statements. Statistics data will be re-read when the tables are re-opened. One way to force all tables to be re-opened is to issue [FLUSH TABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/) command.

A few scenarios where one might need to update statistics tables manually:

- Deleting the statistics. Currently, the [ANALYZE TABLE](/sql-statements-structure/sql-statements/table-statements/analyze-table/) command will collect the statistics, but there is no special command to delete statistics.
- Running ANALYZE on a different server. ANALYZE TABLE does a full table scan, which can put too much load on the server.  It is possible to run ANALYZE on the slave, and then take the data from statistics tables on the slave and apply it on the master.
- In some cases, knowledge of the database allows one to compute statistics manually in a more efficient way than ANALYZE does. One can compute the statistics manually and put it into the database.

## See Also

- [Index Statistics](/replication/optimization-and-tuning/optimization-and-indexes/index-statistics/)
- [InnoDB Persistent Statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/innodb-persistent-statistics/)
- [Histogram-based Statistics](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/histogram-based-statistics/)