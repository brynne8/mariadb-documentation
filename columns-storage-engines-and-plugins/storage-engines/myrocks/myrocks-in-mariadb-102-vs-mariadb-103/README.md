# MyRocks in MariaDB 10.2 vs MariaDB 10.3

MyRocks storage engine itself is identical in [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and [MariaDB 10.3](/kb/en/what-is-mariadb-103/).

[MariaDB 10.3](/kb/en/what-is-mariadb-103/) has a feature that should be interesting for MyRocks users. It is the [gtid_pos_auto_engines](/kb/en/gtid/#gtid_pos_auto_engines) option ([MDEV-12179](https://jira.mariadb.org/browse/MDEV-12179)). This is a performance feature for replication slaves that use multiple transactional storage engines.

For further information, see [mysql.gtid_slave_pos table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlgtid_slave_pos-table/).