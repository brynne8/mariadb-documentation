# InnoDB

##### MariaDB starting with [10.3.7](/kb/en/mariadb-1037-release-notes/)

In [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/) and later, the InnoDB implementation has diverged substantially from the InnoDB in MySQL. Therefore, in these versions, the InnoDB version is no longer associated with a MySQL release version.

##### MariaDB starting with [10.2](/kb/en/what-is-mariadb-102/)

In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, the default InnoDB implementation is based on InnoDB from MySQL 5.7. See [Why MariaDB uses InnoDB instead of XtraDB from MariaDB 10.2](/kb/en/why-does-mariadb-102-use-innodb-instead-of-xtradb/) for more information.

##### MariaDB until [10.1](/kb/en/what-is-mariadb-101/)

In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and before, the default InnoDB implementation is based on Percona's XtraDB. XtraDB is a performance enhanced fork of InnoDB. For compatibility reasons, the [system variables](/kb/en/xtradbinnodb-server-system-variables/) still retain their original `innodb` prefixes. If the documentation says that something applies to InnoDB, then it usually also applies to the XtraDB fork, unless explicitly stated otherwise. In these versions, it is still possible to use InnoDB instead of XtraDB. See [Using InnoDB instead of XtraDB](/columns-storage-engines-and-plugins/storage-engines/innodb/using-innodb-instead-of-xtradb) for more information.

- [InnoDB Versions](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-versions/) — From MariaDB 10.2, InnoDB is the default storage engine.
- [About XtraDB](/columns-storage-engines-and-plugins/storage-engines/innodb/about-xtradb/) — XtraDB was an enhanced version of the InnoDB storage engine used until MariaDB 10.2.
- [InnoDB Limitations](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-limitations/) — The InnoDB storage engine has the following limitations.
- [InnoDB Troubleshooting](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-troubleshooting/) — Guidelines when troubleshooting problems with InnoDB (or XtraDB).
- [InnoDB System Variables](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-system-variables/) — List and description of XtraDB/InnoDB-related server system variables.
- [InnoDB Server Status Variables](/replication/optimization-and-tuning/system-variables/innodb-status-variables/) — List and description of InnoDB/XtraDB status variables.
- [AUTO_INCREMENT Handling in InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/auto_increment-handling-in-innodb/) — AUTO_INCREMENT handling in InnoDB and the lock modes.
- [InnoDB Buffer Pool](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-buffer-pool/) — The most important memory buffer used by InnoDB.
- [InnoDB Change Buffering](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-change-buffering/) — Buffering INSERT, UPDATE and DELETE  statements for greater efficiency.
- [InnoDB Doublewrite Buffer](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-doublewrite-buffer/) — Buffer used for recovering from half-written pages.
- [InnoDB Tablespaces](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/) — Information on tablespaces in InnoDB, including an overview, system tablesp...
- [InnoDB File Format](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-file-format/) — Description of the file formats supported by XtraDB/InnoDB.
- [InnoDB Row Formats](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/) — InnoDB's row formats are REDUNDANT, COMPACT, DYNAMIC, and COMPRESSED.
- [InnoDB Strict Mode](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-strict-mode/) — InnoD strict mode makes InnoDB more reliable.
- [InnoDB Redo Log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-redo-log/) — The redo log is used by InnoDB during crash recovery.
- [InnoDB Undo Log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-undo-log/) — InnoDB Undo log.
- [InnoDB Page Flushing](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-page-flushing/) — Configuring when and how InnoDB flushes dirty pages to disk.
- [InnoDB Purge](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-purge/) — When a transaction updates a row in an InnoDB table, InnoDB's MVCC impleme...
- [Information Schema InnoDB Tables](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-innodb-tables/) — All InnoDB-specific Information Schema tables.
- [Information Schema XtraDB Tables](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-xtradb-tables/) — All XtraDB-specific Information Schema tables.
- [InnoDB Online DDL](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-online-ddl/) — InnoDB tables support online DDL in certain circumstances.
- [Binary Log Group Commit and InnoDB Flushing Performance](/columns-storage-engines-and-plugins/storage-engines/innodb/binary-log-group-commit-and-innodb-flushing-performance/) — Improvement for group commit for InnoDB transactions with the binary log enabled
- [InnoDB Page Compression](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-page-compression/) — InnoDB page compression, which is more sophisticated than the COMPRESSED row format.
- [InnoDB Data Scrubbing](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-data-scrubbing/) — Ensuring data is completely removed when deleted.
- [InnoDB Lock Modes](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-lock-modes/) — InnoDB supports a number of lock modes to ensure that concurrent write operations never collide.
- [InnoDB Monitors](/columns-storage-engines-and-plugins/storage-engines/innodb/xtradb-innodb-monitors/) — Standard Monitor, Lock Monitor, Tablespace Monitor and the Table Monitor.
- [InnoDB Encryption Overview](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/innodb-encryption/innodb-encryption-overview/) — Data-at-rest encryption for tables that use the InnoDB and XtraDB storage engines.
- [Using InnoDB Instead of XtraDB](/columns-storage-engines-and-plugins/storage-engines/innodb/using-innodb-instead-of-xtradb/) — Using the InnoDB plugin instead of XtraDB