# MariaDB ColumnStore 1.0.2 Alpha Release Notes

<strong>Release date:</strong> 23 August 2016

[MariaDB ColumnStore 1.0.2](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore) is an alpha release of MariaDB ColumnStore. This is the second alpha release of  MariaDB ColumnStore with improvements over previous alpha release of 1.0.1.

MariaDB ColumnStore 1.0.2 is an <strong><em>[Alpha](/kb/en/release-criteria/)</em></strong> release.

<strong>Do not use <em>alpha</em> releases on production systems!</strong>

For an overview of [MariaDB ColumnStore](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore) see [MariaDB ColumnStore Architectural Overview](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-architecture/columnstore-architectural-overview)

MariaDB ColumnStore 1.0.2 is an Alpha release. This is the second alpha release of  MariaDB ColumnStore with improvements over previous alpha release of 1.0.1.

Please provide feedback in [JIRA](https://jira.mariadb.org/browse/MCOL) for anything that is not working as expected so that we can fix it before we make the release available for the larger community.
For general "how to questions" ask questions [here](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore) or subscribe to mariadb-columnstore@googlegroups.com

## Notable Changes

- [MCOL-70](https://jira.mariadb.org/browse/MCOL-70): systemd service support
- [MCOL-4](https://jira.mariadb.org/browse/MCOL-4) Build for CentOS 7
- [MCOL-5](https://jira.mariadb.org/browse/MCOL-5) Build for Ubuntu 16.04
- [MCOL-58](https://jira.mariadb.org/browse/MCOL-58) Non-root installs now works for columnstore mysql code

## Bugs and Issues Fixed

Below is list of some of the bugs and issues fixed. For the complete list please see [here](https://jira.mariadb.org/issues/?jql=project%20%3D%20%22MCOL%22%20AND%20status%20%3D%20CLOSED%20and%20fixVersion%20in%20(1.0.2)%20ORDER%20BY%20priority%20DESC%2C%20updated%20DESC)

- [MCOL-35](https://jira.mariadb.org/browse/MCOL-35), [MCOL-66](https://jira.mariadb.org/browse/MCOL-66),  [MCOL-14](https://jira.mariadb.org/browse/MCOL-14): Concurrent DML and DDL failed
- [MCOL-140](https://jira.mariadb.org/browse/MCOL-140): Concurrent Insert threads generate errors
- [MCOL-254](https://jira.mariadb.org/browse/MCOL-254): ColumnStore is limited to two columns in a GROUP BY statement
- [MCOL-67](https://jira.mariadb.org/browse/MCOL-67): config.log and possibly other autoconf artifacts are in source control
- [MCOL-69](https://jira.mariadb.org/browse/MCOL-69): autotools based build process does not work
- [MCOL-151](https://jira.mariadb.org/browse/MCOL-151): Columnstore engine not available after reboot
- [MCOL-39](https://jira.mariadb.org/browse/MCOL-39): console command "getSystemConfig" return API for some parameter names
- [MCOL-59](https://jira.mariadb.org/browse/MCOL-59): name change of Calpont.xml files
- [MCOL-71](https://jira.mariadb.org/browse/MCOL-71): Problem changing the MariaDB release version in Columnstore build
- [MCOL-113](https://jira.mariadb.org/browse/MCOL-113): install_calpont_mysql.sh returns function not found errors on a new installation
- [MCOL-138](https://jira.mariadb.org/browse/MCOL-138): postConfigure mentions MySQL
- [MCOL-139](https://jira.mariadb.org/browse/MCOL-139): MariaDB ColumnStore version confusion
- [MCOL-146](https://jira.mariadb.org/browse/MCOL-146): build enterprise rpms from new build processes
- [MCOL-68](https://jira.mariadb.org/browse/MCOL-68): Makefile.in missing in oamapps/mcsadmin/
- [MCOL-64](https://jira.mariadb.org/browse/MCOL-64): Internal error: CAL0009: Error in dropping table from systables

## Upgrade workaround

With the fixing of [MCOL-59](https://jira.mariadb.org/browse/MCOL-59), the MariaDB Columnstore Platform Configuration file name has changed between 1.0.1 and 1.0.2. So if you have a 1.0.0 or 1.0.1 system and you want to upgrade to 1.0.2, you will need to do the following as part of the upgrade process. If you decide to reinstall 1.0.2 from scratch, this is isn’t required.

For details on how to execute the workaround please see [https://mariadb.com/kb/en/mariadb/upgrading-mariadb-columnstore-from-101-to-102/](https://mariadb.com/kb/en/mariadb/upgrading-mariadb-columnstore-from-101-to-102/)

## Known Issues and Limitations

There are a number bugs and known limitations within this early Alpha version of MariaDB ColumnStore, the most serious of these are listed below. These are expected to be fixed way before the Beta release.
There are some known security issues. They are listed [here](https://mariadb.com/kb/en/mariadb/security-vulnerabilities-mariadb-columnstore/)

- [MCOL-82](https://jira.mariadb.org/browse/MCOL-82): Subquery using IN with VIEW returns incorrect results. Queries selecting from view and using IN in where clause with a subquery on another view returns incorrect results.
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
- [MCOL-259](https://jira.mariadb.org/browse/MCOL-259): After starting MariaDB ColumnStore Service, if queries start within few nano-seconds, queries fail with “At least one DBRoot required for that query is offline“ message
<ul><li>Wait for 1 minute after the service is started, before starting to send queries
</li></ul>
- [MCOL-271](https://jira.mariadb.org/browse/MCOL-271) and [MCOL-171](https://jira.mariadb.org/browse/MCOL-171): empty string and date/datetime values are treated as NULL. This means you cannot insert empty values into a NOT NULL column.
- [MCOL-256](https://jira.mariadb.org/browse/MCOL-256): Queries that have inline comments will produce erroneous results
- While Millisecond and Microsecond storage is supported for datetime, time and timestamp columns, at this time the query results cannot return millisecond and microseconds.
- UTF-8 Limitation
<ul start="1"><li>UTF-8 must be declared at the table level if the instance has been set up with a UTF-8 profile. Tables created with a non-matching character set will yield indeterminate results. 
</li><li>Viewing SQL output should be done using client software that supports UTF-8 character sets. 
</li><li>UTF-8 characters are not supported in object names. 
</li></ul>

## Documentation

[MariaDB ColumnStore Documentation](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore)

## Packaging

RPM and binary packages are provided for the Linux distributions supported by MariaDB ColumnStore 1.0.2 Alpha version.

- The supported OS for this Alpha version are CentOS 6, CentOS 7 and Ubuntu 16.0.4.
- Packages can be downloaded [here](https://mariadb.com/my_portal/download/mariadb-columnstore)

## Source Code

The source code of MariaDB ColumnStore is tagged at GitHub with a tag, which is identical with the version of MariaDB ColumnStore. For instance, the tag of version X.Y.Z of MariaDB ColumnStore is columnstore-X.Y.Z. Further, master always refers to the latest released non-beta version.
The source code is available at these locations

- Storage Engine - [Source code for engine specific processes on UM and PM node](https://github.com/mariadb-corporation/mariadb-columnstore-engine)
- MariaDB Server - [Source code based on MariaDB Server 10.1 modified to support the ColumnStore storage engine](https://github.com/mariadb-corporation/mariadb-columnstore-server)