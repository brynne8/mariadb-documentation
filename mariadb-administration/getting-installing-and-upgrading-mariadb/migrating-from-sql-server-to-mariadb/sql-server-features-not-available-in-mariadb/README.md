# SQL Server Features Not Available in MariaDB

When planning a migration between different DBMSs, one of the most important aspects to consider is that the new database system will probably miss some features supported by the old one. This is not relevant for all users. The most widely used features are supported by most DBMSs. However, it is important to make a list of unsupported features and check which of them are currently used by applications. In most cases it is possible to implement such features on the application side, or simply stop using them.

This page has a list of SQL Server features that are not supported in MariaDB. The list is not exhaustive.

## Introduced in SQL Server versions older than 2016

- Full outer joins.
- `GROUP BY CUBE` syntax.
- `MERGE` statement.
- In MariaDB, indexes are always ascending. Defining them as `ASC` or `DESC` has no effect.
<ul start="1"><li>For single-column indexes, the performance difference between an `ORDER BY ... ASC` and `DESC` is negligible.
</li><li>For multiple-column indexes, an index may be unusable for certain queries because `DESC` is not supported. In some cases, a [generated column](/sql-statements-structure/sql-statements/data-definition/create/generated-columns/) can be used to invert the order of an index (for example, the expression `0 - price` can be indexed to index the prices in a descending order).
</li></ul>
- The [WITH](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/common-table-expressions/with/) syntax is currently only supported for the `SELECT` statement.
- Filtered indexes (`CREATE INDEX ... WHERE`).
- Autonomous transactions.
- User-defined types.
- Rules.
- [Triggers](/programming-customizing-mariadb/triggers-events/triggers/) don't support the following features:
<ul start="1"><li>Triggers on DDL and login.
</li><li>`INSTEAD OF` triggers.
</li><li>The `DISABLE TRIGGER` syntax.
</li></ul>
- [Cursors](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-cursors/) advanced features.
<ul start="1"><li>Global cursors.
</li><li>`DELETE ... CURRENT OF`, `UPDATE ... CURRENT OF` statements: MariaDB cursors are read-only.
</li><li>Specifying a direction (MariaDB cursors can only advance by one row).
</li></ul>
- Synonyms.
- Table variables.
- Queues.
- XML indexes, XML schema collection, XQuery.
- User access to system functionalities, for example:
<ul start="1"><li>Running system commands (`xp_cmdshell()`).
</li><li>Sending emails (`sp_send_dbmail()`).
</li><li>Sending HTTP requests.
</li></ul>
- External languages, external libraries (MariaDB only supports procedural SQL and PL/SQL).
- Negative permissions (the `DENY` command).
- Snapshot replication. See [Provisioning a Slave](/kb/en/mariadb-replication-overview-for-sql-server-users/#provisioning-a-slave).

## Introduced in SQL Server 2016

- Native data masking
- PolyBase (however, [MariaDB 10.5](/kb/en/what-is-mariadb-105/) supports accessing Amazon S3 via the [S3 storage engine](/columns-storage-engines-and-plugins/storage-engines/s3-storage-engine/) and several DBMSs via [CONNECT](/columns-storage-engines-and-plugins/storage-engines/connect/))
- R and Python services
- ColumnStore indexes. MariaDB has a storage engine called [ColumnStore](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/), but this is a completely different feature.

## Introduced in SQL Server 2017

- Adaptive joins
- Graph SQL

## See Also

- [SQL Server Features Implemented Differently in MariaDB](/mariadb-administration/getting-installing-and-upgrading-mariadb/migrating-from-sql-server-to-mariadb/sql-server-features-implemented-differently-in-mariadb/)