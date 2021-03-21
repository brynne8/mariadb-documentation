# MyRocks

MyRocks is a storage engine that adds the RocksDB database to MariaDB. RocksDB is an LSM database with a great compression ratio that is optimized for flash storage.

<table><tbody><tr><th>MyRocks Version</th><th>Introduced</th><th>Maturity</th></tr>
<tr><td>MyRocks  1.0</td><td><a href="/kb/en/mariadb-1037-release-notes/">MariaDB 10.3.7</a>, <a href="/kb/en/mariadb-10216-release-notes/">MariaDB 10.2.16</a></td><td>Stable</td></tr>
<tr><td>MyRocks  1.0</td><td><a href="/kb/en/mariadb-1035-release-notes/">MariaDB 10.3.5</a>, <a href="/kb/en/mariadb-10214-release-notes/">MariaDB 10.2.14</a></td><td>Gamma</td></tr>
<tr><td>MyRocks  1.0</td><td><a href="/kb/en/mariadb-1034-release-notes/">MariaDB 10.3.4</a>, <a href="/kb/en/mariadb-10213-release-notes/">MariaDB 10.2.13</a></td><td>Beta</td></tr>
<tr><td>MyRocks  1.0</td><td><a href="/kb/en/mariadb-1025-release-notes/">MariaDB 10.2.5</a></td><td>Alpha</td></tr>
</tbody></table>

- [About MyRocks for MariaDB](/columns-storage-engines-and-plugins/storage-engines/myrocks/about-myrocks-for-mariadb/) — Enables greater compression than InnoDB, and less write amplification.
- [Getting Started with MyRocks](/columns-storage-engines-and-plugins/storage-engines/myrocks/getting-started-with-myrocks/) — Installing and getting started with MyRocks.
- [Building MyRocks in MariaDB](/columns-storage-engines-and-plugins/storage-engines/myrocks/building-myrocks-in-mariadb/) — MariaDB compile process for MyRocks.
- [Loading Data Into MyRocks](/columns-storage-engines-and-plugins/storage-engines/myrocks/loading-data-into-myrocks/) — MyRocks has ways to load data much faster than normal INSERTs
- [MyRocks Status Variables](/columns-storage-engines-and-plugins/storage-engines/myrocks/myrocks-status-variables/) — MyRocks-related status variables.
- [MyRocks System Variables](/columns-storage-engines-and-plugins/storage-engines/myrocks/myrocks-system-variables/) — MyRocks server system variables.
- [MyRocks Transactional Isolation](/columns-storage-engines-and-plugins/storage-engines/myrocks/myrocks-transactional-isolation/) — TODO: 
MyRocks uses snapshot isolation
Support do READ-COMMITTED and REPEAT...
- [MyRocks and Replication](/columns-storage-engines-and-plugins/storage-engines/myrocks/myrocks-and-replication/) — Details about how MyRocks works with replication.
- [MyRocks and Group Commit with Binary log](/columns-storage-engines-and-plugins/storage-engines/myrocks/myrocks-and-group-commit-with-binary-log/) — MyRocks supports group commit with the binary log
- [Optimizer Statistics in MyRocks](/columns-storage-engines-and-plugins/storage-engines/myrocks/optimizer-statistics-in-myrocks/) — How MyRocks provides statistics to the query optimizer
- [Differences Between MyRocks Variants](/columns-storage-engines-and-plugins/storage-engines/myrocks/differences-between-myrocks-variants/) — Differences between Facebook's, MariaDB's and Percona Server's MyRocks.
- [MyRocks and Bloom Filters](/columns-storage-engines-and-plugins/storage-engines/myrocks/myrocks-and-bloom-filters/) — Bloom filters are used to reduce read amplification.
- [MyRocks and CHECK TABLE](/columns-storage-engines-and-plugins/storage-engines/myrocks/myrocks-and-check-table/) — MyRocks supports the CHECK TABLE command.
- [MyRocks and Data Compression](/columns-storage-engines-and-plugins/storage-engines/myrocks/myrocks-and-data-compression/) — MyRocks supports several compression algorithms.
- [MyRocks and Index-Only Scans](/columns-storage-engines-and-plugins/storage-engines/myrocks/myrocks-and-index-only-scans/) — MyRocks and index-only scans on secondary indexes.
- [MyRocks and START TRANSACTION WITH CONSISTENT SNAPSHOT](/columns-storage-engines-and-plugins/storage-engines/myrocks/myrocks-and-start-transaction-with-consistent-snapshot/) — FB/MySQL has added new syntax which returns the binlog coordinates pointing at the snapshot.
- [MyRocks Column Families](/columns-storage-engines-and-plugins/storage-engines/myrocks/myrocks-column-families/) — MyRocks stores data in column families, which are similar to tablespaces.
- [MyRocks in MariaDB 10.2 vs MariaDB 10.3](/columns-storage-engines-and-plugins/storage-engines/myrocks/myrocks-in-mariadb-102-vs-mariadb-103/) — MyRocks storage engine in MariaDB 10.2 and MariaDB 10.3.
- [MyRocks Performance Troubleshooting](/columns-storage-engines-and-plugins/storage-engines/myrocks/myrocks-performance-troubleshooting/) — MyRocks exposes its performance metrics through several interfaces.