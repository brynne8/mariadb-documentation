# MariaDB ColumnStore 1.0.1 Alpha Release Notes

<strong>Release date:</strong> 14 June 2016

[MariaDB ColumnStore 1.0.1](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore) is the development version of MariaDB ColumnStore. It is built by porting InfiniDB 4.6.2 on [MariaDB 10.1.14](/kb/en/mariadb-10114-release-notes/).

MariaDB ColumnStore 1.0.1 is an <strong><em>[Alpha](/kb/en/release-criteria/)</em></strong> release.

<strong>Do not use <em>alpha</em> releases on production systems!</strong>

This is the first MariaDB ColumnStore release, and we are releasing it now to get it into the hands of any one who might want to test it. We plan to do several Alpha releases, each with increased stability and more features.

Note that this is an early Alpha release of [MariaDB ColumnStore](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore) in [MariaDB 10.1.14](/kb/en/mariadb-10114-release-notes/) The porting work from MySQL 5.1 is done but not all features have been fully tested.  This release is only meant for testing, not for using in production!

To understand what to expect from this release, a little background is needed:
[MariaDB ColumnStore](%5B%5Bmariadb-columnstore) is based on the MySQL 5.1 InfiniDB release , which was a stable product with a lot of happy users.  To make this more modern and easier to develop, we moved the InfiniDB code to [MariaDB 10.1.14](/kb/en/mariadb-10114-release-notes/) and renamed it ColumnStore.  As most of the [MariaDB ColumnStore](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore) code is unchanged and as [MariaDB 10.1.14](/kb/en/mariadb-10114-release-notes/) code is stable, the possible problems are mostly in the interface between MariaDB Server and MariaDB ColumnStore.

We have not yet had time to ourselves extensively test all [MariaDB ColumnStore](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore) features and MariaDB ColumnStore has not yet been tested by a larger community, so there may be some issues that needs to be fixed in the next releases.  We do however expect that most things should work and be reasonably stable.

Please provide feedback in [JIRA](https://jira.mariadb.org/browse/MCOL) for anything that is not working as expected so that we can fix it before we make the release available for the larger community.
For general "how to questions" ask questions [here](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore) or subscribe to mariadb-columnstore@googlegroups.com

## Notable Features

- Porting of InfiniDB source code as “columnstore” storage engine on [MariaDB 10.1.14](/kb/en/mariadb-10114-release-notes/)
- All client applications using ODBC, JDBC, MySQL Clients or MariaDB Clients can connect to and query MariaDB ColumnStore
- Windowing Functions Supported: AVG, COUNT, CUME_DIST, DENSE_RANK, MAX, MEDIAN, MIN, NTILE, PERCENT_RANK, PERCENTILE_CONT, PERCENTILE_DISC, RANK, ROW_NUMBER, STDDEV, STDDEV_POP, STDDEV_SAMP, SUM, VARIANCE, VAR_POP, VAR_SAMP
- Bulk Data Load - High speed data loading utility cpimport
- ACID transactions
- MariaDB SQL Syntax compliance (with the exceptions noted in known limitations and issues)
- Cross Engine joins
- Built-in HA

### Changes for old InfiniDB users

- Renaming of the engine to columnstore
- Console command line utility is now called mcsadmin and an alias ma is created upon install for the same
- SQL client utility alias is now called mcsmysql
- Install directory is now under /usr/local/mariadb/columnstore
- Log directory is now under /var/log/mariadb/columnstore
- The numeric data type range values for MariaDB ColumnStore are different from that of other storage engines in MariaDB Server 10.1 as documented here

## Known Issues and Limitations

There are a number bugs and known limitations within this early Alpha version of MariaDB ColumnStore, the most serious of these are listed below. These are expected to be fixed way before the Beta release.
There are some known security issues. They are listed here

- [MCOL-82](https://jira.mariadb.org/browse/MCOL-82): Subquery using IN with VIEW returns incorrect results. Queries selecting from view and using IN in where clause with a subquery on another view returns incorrect results.
- [MCOL-66](https://jira.mariadb.org/browse/MCOL-66),  [MCOL-64](https://jira.mariadb.org/browse/MCOL-64),  [MCOL-14](https://jira.mariadb.org/browse/MCOL-14): When multiple connection tries to perform DDL statements (Create Table/Drop Table/Alter Table) in parallel, system catalog tables get locked returning errors 
<ul start="1"><li>Create Table, Drop Table and Alter Table must be done through a single connection at a time
</li></ul>
- [MCOL-37](https://jira.mariadb.org/browse/MCOL-37): Following three window functions do not return correct value
<ul start="1"><li>FIRST_VALUE
</li><li>LEAD
</li><li>LAG
</li></ul>
- [MCOL-75](https://jira.mariadb.org/browse/MCOL-75) and [MCOL-74](https://jira.mariadb.org/browse/MCOL-74): NTH_VALUE and LAST_VALUE functions return syntax errors.
- [MCOL-73](https://jira.mariadb.org/browse/MCOL-73)): Wide table formatted display causes frontend to return error
<ul start="1"><li>MariaDB ColumnStore supports wide tables storage
</li><li>Displaying the query results on a large number of columns without formatting the column works
</li><li>Displaying the query results on a large number of columns with formatting causes error at MariaDB Server level
</li></ul>
- [MCOL-45](https://jira.mariadb.org/browse/MCOL-45) : CREATE PROCEDURE fails.
<ul start="1"><li>Do not use stored procedure with this Alpha version
[MCOL-16](https://jira.mariadb.org/browse/MCOL-16): LOAD DATA INFILE into datetime columns, data got saturated.
</li><li>Do not use LOAD DATA INFILE when importing data into datetime columns
</li><li>Use cpimport to bulk load data into  MariaDB Columnstore instead. cpimports works correctly for datetime columns as well.
</li><li>INSERT INTO works for datetime columns
</li></ul>
- [MCOL-49](https://jira.mariadb.org/browse/MCOL-49): Drop table does not work if the PM node runs out of space.
- [MCOL-58](https://jira.mariadb.org/browse/MCOL-58):  Non-Root install does not work in this release
<ul start="1"><li>Use root access to install this Alpha version
</li></ul>
- [MCOL-39](https://jira.mariadb.org/browse/MCOL-39): console command "getSystemConfig" returns error for some parameter names
While Millisecond and Microsecond storage is supported for datetime, time and timestamp columns, at this time the query results cannot return millisecond and microseconds.
- UTF-8 Limitation
<ul start="1"><li>UTF-8 must be declared at the table level if the instance has been set up with a UTF-8 profile. Tables created with a non-matching character set will yield indeterminate results. 
</li><li>Viewing SQL output should be done using client software that supports UTF-8 character sets. 
</li><li>UTF-8 characters are not supported in object names. 
</li></ul>

The building of the software needs a special build environment. We're working on making it available for everyone to build.

## Documentation

[MariaDB ColumnStore Documentation](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore) include architecture, getting started, SQL Syntax guide, How to manage and How to load data into MariaDB ColumnStore.

Detailed Installation Guide will be available in next version of the MariaDB ColumnStore 1.0 series

## Packaging

RPM and binary packages are provided for the Linux distributions supported by MariaDB ColumnStore 1.0.1 Alpha version.

- The supported OS for this Alpha version are CentOS 6 and CentOS 7.
- Packages can be downloaded [here](https://mariadb.com/my_portal/download/mariadb-columnstore)

## Source Code

The source code of MariaDB ColumnStore is tagged at GitHub with a tag, which is identical with the version of MariaDB ColumnStore. For instance, the tag of version X.Y.Z of MariaDB ColumnStore is columnstore-X.Y.Z. Further, master always refers to the latest released non-beta version.
The source code is available at these locations

- Storage Engine - [Source code for engine specific processes on UM and PM node](https://github.com/mariadb-corporation/mariadb-columnstore-engine)
- MariaDB Server - [Source code based on MariaDB Server 10.1 modified to support the ColumnStore storage engine](https://github.com/mariadb-corporation/mariadb-columnstore-server)