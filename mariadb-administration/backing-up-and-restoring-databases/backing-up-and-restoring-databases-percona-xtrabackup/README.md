# Percona XtraBackup

In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and later, [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/) is the recommended backup method to use instead of Percona XtraBackup.

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/), Percona XtraBackup is <strong>not supported</strong>. See [Percona XtraBackup Overview: Compatibility with MariaDB](/kb/en/percona-xtrabackup-overview/#compatibility-with-mariadb) for more information.

In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), Percona XtraBackup is only <strong>partially supported</strong>. See [Percona XtraBackup Overview: Compatibility with MariaDB](/kb/en/percona-xtrabackup-overview/#compatibility-with-mariadb) for more information.

Open source tool for performing hot backups of MariaDB, MySQL and Percona Server databases.

- [Percona XtraBackup Overview](/mariadb-administration/backing-up-and-restoring-databases/backing-up-and-restoring-databases-percona-xtrabackup/percona-xtrabackup-overview/) — Open source tool for performing hot backups of MariaDB, MySQL and Percona Server databases.
- [xtrabackup-v2 SST Method](/replication/galera-cluster/state-snapshot-transfers-ssts-in-galera-cluster/xtrabackup-v2-sst-method/) — The xtrabackup-v2 SST method uses the Percona XtraBackup utility for performing SSTs.
- [Manual SST of Galera Cluster Node With Percona XtraBackup](/replication/galera-cluster/state-snapshot-transfers-ssts-in-galera-cluster/manual-sst-of-galera-cluster-node-with-percona-xtrabackup/) — It can be helpful to perform a "manual SST" with Xtrabackup when Galera's normal SSTs fail.
- [Percona XtraBackup Build Instructions](/mariadb-administration/backing-up-and-restoring-databases/backing-up-and-restoring-databases-percona-xtrabackup/percona-xtrabackup-build-instructions/) — Build instructions for Xtrabackup