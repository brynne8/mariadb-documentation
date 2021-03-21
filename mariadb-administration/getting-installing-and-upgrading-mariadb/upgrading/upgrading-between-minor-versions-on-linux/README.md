# Upgrading Between Minor Versions on Linux

For Windows, see [Upgrading MariaDB on Windows](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-mariadb-on-windows/) instead.

For MariaDB Galera Cluster, see [Upgrading Between Minor Versions with Galera Cluster](/replication/galera-cluster/upgrading-galera-cluster/upgrading-between-minor-versions-with-galera-cluster/) instead.

Before you upgrade, it would be best to take a backup of your database. This is always a good idea to do before an upgrade. We would recommend [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/).

To upgrade between minor versions of MariaDB on Linux/Unix (for example from [MariaDB 10.3.12](/kb/en/mariadb-10312-release-notes/) to [MariaDB 10.3.13](/kb/en/mariadb-10313-release-notes/)), the following procedure is suggested:

1 [Stop MariaDB](/kb/en/starting-and-stopping-mariadb-starting-and-stopping-mariadb/).
2 Uninstall the old version of MariaDB.
3 Install the new version of MariaDB.
<ul start="1"><li>On Debian, Ubuntu, and other similar Linux distributions, see [Installing MariaDB Packages with APT](/kb/en/installing-mariadb-deb-files/#installing-mariadb-packages-with-apt) for more information.
</li><li>On RHEL, CentOS, Fedora, and other similar Linux distributions, see [Installing MariaDB Packages with YUM](/kb/en/yum/#installing-mariadb-packages-with-yum) for more information.
</li><li>On SLES, OpenSUSE, and other similar Linux distributions, see [Installing MariaDB Packages with ZYpp](/kb/en/installing-mariadb-with-zypper/#installing-mariadb-packages-with-zypp) for more information.
</li></ul>
4 Make any desired changes to configuration options in [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/), such as `my.cnf`.
5 [Start MariaDB](/kb/en/starting-and-stopping-mariadb-starting-and-stopping-mariadb/).
6 Run [mysql_upgrade](/sql-statements-structure/sql-statements/table-statements/mysql_upgrade/).
<ul start="1"><li>`mysql_upgrade` does two things:
<ol start="1"><li>Ensures that the system tables in the [mysq](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/)l database are fully compatible with the new version.
</li><li>Does a very quick check of all tables and marks them as compatible with the new version of MariaDB .
</li></ol>
</li><li>In most cases this should be a fast operation (depending of course on the number of tables).
</li></ul>

To upgrade between major versions, see the following:

- [Upgrading from MariaDB 10.4 to MariaDB 10.5](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-from-mariadb-104-to-mariadb-105/)
- [Upgrading from MariaDB 10.3 to MariaDB 10.4](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-from-mariadb-103-to-mariadb-104/)
- [Upgrading from MariaDB 10.2 to MariaDB 10.3](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-from-mariadb-102-to-mariadb-103/)
- [Upgrading from MariaDB 10.1 to MariaDB 10.2](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-from-mariadb-101-to-mariadb-102/)
- [Upgrading from MariaDB 10.0 to MariaDB 10.1](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-from-mariadb-100-to-mariadb-101/)
- [Upgrading from MariaDB 5.5 to MariaDB 10.0](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-from-mariadb-55-to-mariadb-100/)