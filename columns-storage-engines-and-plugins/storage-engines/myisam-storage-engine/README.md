# MyISAM

MyISAM was the default [storage engine](storage-engine) from MySQL 3.23 until it was replaced by [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) in MariaDB and MySQL 5.5. It's a light, non-transactional engine with great performance, is easy to copy between systems and has a small data footprint.

You're encouraged to rather use the [Aria](/columns-storage-engines-and-plugins/storage-engines/aria) storage engine for new applications, which has even better performance and the goal of being crash-safe.

Until [MariaDB 10.4](/kb/en/what-is-mariadb-104/), [system tables](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables) used the MyISAM storage engine.

- [MyISAM Overview](/columns-storage-engines-and-plugins/storage-engines/myisam-storage-engine/myisam-overview/) — Light, non-transactional storage engine
- [MyISAM System Variables](/columns-storage-engines-and-plugins/storage-engines/myisam-storage-engine/myisam-system-variables/) — MyISAM system variables.
- [MyISAM Storage Formats](/columns-storage-engines-and-plugins/storage-engines/myisam-storage-engine/myisam-storage-formats/) — The MyISAM storage engine supports three different table storage formats
- [MyISAM Clients and Utilities](/clients-utilities/myisam-clients-and-utilities/) — Clients and utilities for working with MyISAM tables
- [MyISAM Index Storage Space](/columns-storage-engines-and-plugins/storage-engines/myisam-storage-engine/myisam-index-storage-space/) — Regular MyISAM tables make use of B-tree indexes
- [MyISAM Log](/mariadb-administration/server-monitoring-logs/myisam-log/) — Records all changes to MyISAM tables
- [Concurrent Inserts](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/concurrent-inserts/) — Under some circumstances, MyISAM allows INSERTs and SELECTs to be executed concurrently.
- [Segmented Key Cache](/replication/optimization-and-tuning/system-variables/segmented-key-cache/) — Collection of structures for regular MyISAM key caches