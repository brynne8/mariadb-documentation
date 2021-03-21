# Choosing the Right Storage Engine

A high-level overview of the main reasons for choosing a particular storage engine:

## Topic List

### General Purpose

- [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) is a good general transaction storage engine, and, from [MariaDB 10.2](/kb/en/what-is-mariadb-102/), the best choice in most cases. It is the default storage engine from [MariaDB 10.2](/kb/en/what-is-mariadb-102/). For earlier releases, XtraDB was a performance enhanced fork of InnoDB and is usually preferred.
- [XtraDB](/kb/en/xtradb/) is the best choice in [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and earlier in the majority of cases. It is a performance-enhanced fork of InnoDB and is MariaDB's default engine until [MariaDB 10.1](/kb/en/what-is-mariadb-101/).
- [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/), MariaDB's more modern improvement on [MyISAM](/kb/en/myisam/), has a small footprint and allows for easy copying between systems.
- [MyISAM](/kb/en/myisam/) has a small footprint and allows for easy copying between systems. MyISAM is MySQL's oldest storage engine. There is usually little reason to use it except for legacy purposes. Aria is MariaDB's more modern improvement.

### Scaling, Partitioning

When you want to split your database load on several servers or optimize for scaling. We also suggest looking at [Galera](/kb/en/galera/), a synchronous multi-master cluster.

- [Spider](/columns-storage-engines-and-plugins/storage-engines/spider/) uses partitioning to provide data sharding through multiple servers.
- [ColumnStore](/kb/en/columnstore/) utilizes a massively parallel distributed data architecture and is designed for big data scaling to process petabytes of data.
- The [MERGE](/columns-storage-engines-and-plugins/storage-engines/merge/) storage engine is a collection of identical [MyISAM](/kb/en/myisam/) tables that can be used as one. "Identical" means that all tables have identical column and index information.
- [TokuDB](/columns-storage-engines-and-plugins/storage-engines/tokudb/) is a transactional storage engine which is optimized for workloads that do not fit in memory, and provides a good compression ratio. TokuDB has been deprecated by its upstream developers, and is disabled in [MariaDB 10.5](/kb/en/what-is-mariadb-105/), and removed in [MariaDB 10.6](/kb/en/what-is-mariadb-106/)

### Compression / Archive

- [MyRocks](/columns-storage-engines-and-plugins/storage-engines/myrocks/) enables greater compression than InnoDB, as well as less  write amplification giving better endurance of flash storage and improving overall throughput.
- The [Archive](/columns-storage-engines-and-plugins/storage-engines/archive/) storage engine is, unsurprisingly, best used for archiving.
- [TokuDB](/columns-storage-engines-and-plugins/storage-engines/tokudb/) is a transactional storage engine which is optimized for workloads that do not fit in memory, and provides a good compression ratio. TokuDB has been deprecated by its upstream developers, and is disabled in [MariaDB 10.5](/kb/en/what-is-mariadb-105/), and removed in [MariaDB 10.6](/kb/en/what-is-mariadb-106/)

### Connecting to Other Data Sources

When you want to use data not stored in a MariaDB database.

- [CONNECT](/columns-storage-engines-and-plugins/storage-engines/connect/) allows access to different kinds of text files and remote resources as if they were regular MariaDB tables.
- The [CSV](/columns-storage-engines-and-plugins/storage-engines/csv/) storage engine can read and append to files stored in CSV (comma-separated-values) format. However, since [MariaDB 10.0](/kb/en/what-is-mariadb-100/), CONNECT is a better choice and is more flexibly able to read and write such files.
- [FederatedX](/kb/en/federatedx/) uses libmysql to talk to the data source, the data source being a remote RDBMS. Currently, since FederatedX only uses libmysql, it can only talk to another MySQL RDBMS.
- [CassandraSE](/kb/en/cassandrase/) is a storage engine allowing access to an older version of Apache Cassandra NoSQL DBMS. It was relatively experimental, is no longer being actively developed and has been removed in [MariaDB 10.6](/kb/en/what-is-mariadb-106/).

### Search Optimized

Search engines optimized for search.

- [SphinxSE](/kb/en/sphinxse/) is used as a proxy to run statements on a remote Sphinx database server (mainly useful for advanced fulltext searches).
- [Mroonga](/columns-storage-engines-and-plugins/storage-engines/mroonga/) provides fast CJK-ready full text searching using column store.

### Cache, Read-only

- [MEMORY](/replication/optimization-and-tuning/query-optimizations/guiduuid-performance/mariadb/memory-storage-engine/) does not write data on-disk (all rows are lost on crash) and is best-used for read-only caches of data from other tables, or for temporary work areas. With the default [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) and other storage engines having good caching, there is less need for this engine than in the past.

### Other Specialized Storage Engines

- [S3 Storage Engine](/columns-storage-engines-and-plugins/storage-engines/s3-storage-engine/) is a read-only storage engine that stores its data in Amazon S3.
- [Sequence](/kb/en/sequence/) allows the creation of ascending or descending sequences of numbers (positive integers) with a given starting value, ending value and increment, creating virtual, ephemeral tables automatically when you need them.
- The [BLACKHOLE](/columns-storage-engines-and-plugins/storage-engines/blackhole/) storage engine accepts data but does not store it and always returns an empty result. This can be useful in [replication](/replication/) environments, for example, if you want to run complex filtering rules on a slave without incurring any overhead on a master.
- [OQGRAPH](/kb/en/oqgraph/) allows you to handle hierarchies (tree structures) and complex graphs (nodes having many connections in several directions).

## Alphabetical List

- The [Archive](/columns-storage-engines-and-plugins/storage-engines/archive/) storage engine is, unsurprisingly, best used for archiving.
- [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/), MariaDB's more modern improvement on MyISAM, has a small footprint and allows for easy copy between systems.
- The [BLACKHOLE](/columns-storage-engines-and-plugins/storage-engines/blackhole/) storage engine accepts data but does not store it and always returns an empty result. This can be useful in [replication](/replication/) environments, for example, if you want to run complex filtering rules on a slave without incurring any overhead on a master.
- [CassandraSE](/kb/en/cassandrase/) is a storage engine allowing access to an older version of Apache Cassandra NoSQL DBMS. It was relatively experimental, is no longer being actively developed and has been removed in [MariaDB 10.6](/kb/en/what-is-mariadb-106/).
- [ColumnStore](/kb/en/columnstore/) utilizes a massively parallel distributed data architecture and is designed for big data scaling to process petabytes of data.
- [CONNECT](/columns-storage-engines-and-plugins/storage-engines/connect/) allows access to different kinds of text files and remote resources as if they were regular MariaDB tables.
- The [CSV](/columns-storage-engines-and-plugins/storage-engines/csv/csv-overview/) storage engine can read and append to files stored in CSV (comma-separated-values) format. However, since [MariaDB 10.0](/kb/en/what-is-mariadb-100/), CONNECT is a better choice and is more flexibly able to read and write such files.
- [FederatedX](/kb/en/federatedx/) uses libmysql to talk to the data source, the data source being a remote RDBMS. Currently, since FederatedX only uses libmysql, it can only talk to another MySQL RDBMS.
- [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) is a good general transaction storage engine, and, from [MariaDB 10.2](/kb/en/what-is-mariadb-102/), the best choice in most cases. It is the default storage engine from [MariaDB 10.2](/kb/en/what-is-mariadb-102/). For earlier releases, XtraDB was a performance enhanced fork of InnoDB and is usually preferred.
- The [MERGE](/columns-storage-engines-and-plugins/storage-engines/merge/) storage engine is a collection of identical MyISAM tables that can be used as one. "Identical" means that all tables have identical column and index information.
- [MEMORY](/replication/optimization-and-tuning/query-optimizations/guiduuid-performance/mariadb/memory-storage-engine/) does not write data on-disk (all rows are lost on crash) and is best-used for read-only caches of data from other tables, or for temporary work areas. With the default [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) and other storage engines having good caching, there is less need for this engine than in the past.
- [Mroonga](/columns-storage-engines-and-plugins/storage-engines/mroonga/) provides fast CJK-ready full text searching using column store.
- [MyISAM](/kb/en/myisam/) has a small footprint and allows for easy copying between systems. MyISAM is MySQL's oldest storage engine. There is usually little reason to use it except for legacy purposes. Aria is MariaDB's more modern improvement.
- [MyRocks](/columns-storage-engines-and-plugins/storage-engines/myrocks/) enables greater compression than InnoDB, as well as less  write amplification giving better endurance of flash storage and improving overall throughput.
- [OQGRAPH](/kb/en/oqgraph/) allows you to handle hierarchies (tree structures) and complex graphs (nodes having many connections in several directions).
- [S3 Storage Engine](/columns-storage-engines-and-plugins/storage-engines/s3-storage-engine/) is a read-only storage engine that stores its data in Amazon S3.
- [Sequence](/kb/en/sequence/) allows the creation of ascending or descending sequences of numbers (positive integers) with a given starting value, ending value and increment, creating virtual, ephemeral tables automatically when you need them.
- [SphinxSE](/columns-storage-engines-and-plugins/storage-engines/sphinx-storage-engine/) is used as a proxy to run statements on a remote Sphinx database server (mainly useful for advanced fulltext searches).
- [Spider](/columns-storage-engines-and-plugins/storage-engines/spider/) uses partitioning to provide data sharding through multiple servers.
- [TokuDB](/columns-storage-engines-and-plugins/storage-engines/tokudb/) is a transactional storage engine which is optimized for workloads that do not fit in memory, and provides a good compression ratio. TokuDB has been deprecated by its upstream developers, and is disabled in [MariaDB 10.5](/kb/en/what-is-mariadb-105/), and removed in [MariaDB 10.6](/kb/en/what-is-mariadb-106/)
- [XtraDB](/kb/en/xtradb/) is the best choice in [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and earlier in the majority of cases. It is a performance-enhanced fork of InnoDB and is MariaDB's default engine until [MariaDB 10.1](/kb/en/what-is-mariadb-101/).