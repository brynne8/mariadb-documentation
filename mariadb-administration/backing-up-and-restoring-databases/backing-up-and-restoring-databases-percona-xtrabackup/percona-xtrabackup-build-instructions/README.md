# Percona XtraBackup Build Instructions

In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and later, [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/) is the recommended backup method to use instead of Percona XtraBackup.

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/), Percona XtraBackup is <strong>not supported</strong>. See [Percona XtraBackup Overview: Compatibility with MariaDB](/kb/en/percona-xtrabackup-overview/#compatibility-with-mariadb) for more information.

In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), Percona XtraBackup is only <strong>partially supported</strong>. See [Percona XtraBackup Overview: Compatibility with MariaDB](/kb/en/percona-xtrabackup-overview/#compatibility-with-mariadb) for more information.

Build instructions for [Percona XtraBackup](/kb/en/percona-xtrabackup/).

Solaris 10 (SunOS 5.10) notes:

Edit utils/build.sh and add -lrt -m64 to CFLAGS and CXXFLAGS.

Make sure that you're using GNU utils for building, including make, cmake, gcc, gawk, getopt, autotools, libtool, automake, autoconf and bazaar.

If you want to change MySQL version which to build against with edit one of the following lines:

MYSQL_51_VERSION=...

MYSQL_55_VERSION=...

PS_51_VERSION=...

PS_55_VERSION=..

When ready run one of the following depending on the MySQL version which you want to build with:

AUTO_DOWNLOAD="yes" ./utils/build.sh xtradb (build against XtraDB 5.1)

AUTO_DOWNLOAD="yes" ./utils/build.sh innodb51_builtin (build against built-in InnoDB in MySQL 5.1)

AUTO_DOWNLOAD="yes" ./utils/build.sh xtradb55 (build against XtraDB 5.5)

AUTO_DOWNLOAD="yes" ./utils/build.sh innodb55 (build against InnoDB in MySQL 5.5)