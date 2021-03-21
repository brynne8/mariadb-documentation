# About MariaDB ColumnStore

MariaDB ColumnStore is a columnar storage engine that utilizes a massively parallel distributed data architecture. It's a columnar storage system built by porting InfiniDB 4.6.7 to MariaDB, and released under the GPL license.

From [MariaDB 10.5.4](/kb/en/mariadb-1054-release-notes/), it is available as a storage engine for MariaDB Server. Before then, it is only available as a separate download.

Documentation for the latest release of Columnstore is not available on the Knowledge Base. Instead, see:

- [Release Notes](https://mariadb.com/docs/release-notes/mariadb-columnstore-1-5-2-release-notes/)
- [Deployment Instructions](https://mariadb.com/docs/deploy/community-single-columnstore/)

It is designed for big data scaling to process petabytes of data, linear scalability and exceptional performance with real-time response to analytical queries. It leverages the I/O benefits of columnar storage, compression, just-in-time projection, and horizontal and vertical partitioning to deliver tremendous performance when analyzing large data sets.

Links:

- [MariaDB Columnstore Blogs](https://mariadb.com/resources/blog/tag/columnstore/).
- A Google Group exists for MariaDB ColumnStore that can be used to discuss ideas, issues and communicate with the  community: Send email to mariadb-columnstore@googlegroups.com or use the [forum interface](https://groups.google.com/forum/#!forum/mariadb-columnstore)
- Bugs can be reported in MariaDB Jira: [https://jira.mariadb.org](https://jira.mariadb.org) (see [Reporting Bugs](/kb/en/reporting-bugs/)). Please file bugs under the MCOL project and include the output from the [support utility](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-troubleshooting/system-troubleshooting-mariadb-columnstore) if possible.

MariaDB ColumnStore is released under the GPL license.