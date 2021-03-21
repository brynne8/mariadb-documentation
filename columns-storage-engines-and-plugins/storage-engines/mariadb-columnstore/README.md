# MariaDB ColumnStore

MariaDB ColumnStore is a columnar storage engine that utilizes a massively parallel distributed data architecture. It's a columnar storage system built by porting InfiniDB 4.6.7 to MariaDB, and released under the GPL license.

From [MariaDB 10.5.4](/kb/en/mariadb-1054-release-notes/), it is available as a storage engine for MariaDB Server. Before then, it is only available as a separate download.

MariaDB ColumnStore is designed for big data scaling to process petabytes of data, linear scalability and exceptional performance with real-time response to analytical queries. It leverages the I/O benefits of columnar storage, compression, just-in-time projection, and horizontal and vertical partitioning to deliver tremendous performance when analyzing large data sets.

Documentation for the latest release of Columnstore is not available on the Knowledge Base. Instead, see:

- [Release Notes](https://mariadb.com/docs/release-notes/mariadb-columnstore-1-5-2-release-notes/)
- [Deployment Instructions](https://mariadb.com/docs/deploy/community-single-columnstore/)

- [About MariaDB ColumnStore](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/about-mariadb-columnstore/) — About MariaDB ColumnStore.
- [MariaDB ColumnStore Release Notes](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-release-notes/) — MariaDB ColumnStore Release Notes
- [ColumnStore Getting Started](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-getting-started/) — Quick summary of steps needed to install MariaDB ColumnStore
- [ColumnStore Upgrade Guides](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/mariadb-columnstore-columnstore/) — Documentation on upgrading from prior versions and InfiniDB migration.
- [ColumnStore Architecture](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-architecture/) — MariaDB ColumnStore Architecture
- [Managing ColumnStore](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/managing-columnstore/) — Managing MariaDB ColumnStore System Environment and Database
- [ColumnStore Data Ingestion](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-data-ingestion/) — How to load and manipulate data into MariaDB ColumnStore
- [ColumnStore SQL Structure and Commands](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-sql-structure-and-commands/) — SQL syntax supported by MariaDB ColumnStore
- [ColumnStore Performance Tuning](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-performance-tuning/) — Information relating to configuring and analyzing the ColumnStore system for optimal performance.
- [ColumnStore System Variables](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-system-variables/) — ColumnStore System Variables
- [ColumnStore Security Vulnerabilities](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-security-vulnerabilities/) — Security vulnerabilities affecting MariaDB ColumnStore
- [ColumnStore Troubleshooting](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-troubleshooting/) — Articles on troubleshooting tips and techniques
- [Using MariaDB ColumnStore](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/using-mariadb-columnstore/) — Provides details on using third party products and tools with MariaDB ColumnStore
- [Building ColumnStore in MariaDB](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/building-columnstore-in-mariadb/) — This is a description of how to build and start a local ColumnStore install...
- [Can't create a table starting with a capital letter. All tables are lower case-](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/mariadb-columnstore-cant-create-a-table-starting-with-a-capital-letter-all-/) — Hi,
I was playing around with my MariaDB ColumnStore and I noticed the I am...
- [Sample storagemanager.cnf](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/sample-storagemanagercnf/) — Sample storagemanager.cnf

[ObjectStorage]
service = S3
object_size = 5M
metadata_path = /v
- [Using Storage Manager with IAM Role](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/using-storage-manager-with-iam-role/) — AWS IAM Role Configuration
We have added a new feature in Columnstore 5.5.2...