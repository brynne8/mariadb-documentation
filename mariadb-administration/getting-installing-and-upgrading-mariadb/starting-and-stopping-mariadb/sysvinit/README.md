# sysVinit

[sysVinit](https://en.wikipedia.org/wiki/Init#SysV-style) is one of the most common service managers. On systems that use [sysVinit](https://en.wikipedia.org/wiki/Init#SysV-style), the [mysql.server](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqlserver/) script is normally installed to `/etc/init.d/mysql`.

## Interacting with the MariaDB Server Process

The service can be interacted with by using the <a undefined>service</a> command.

### Starting the MariaDB Server Process on Boot

On RHEL/CentOS and other similar distributions, the <a undefined>chkconfig</a> command can be used to enable the MariaDB Server process at boot:

```sql
chkconfig --add mysql
chkconfig --level 345 mysql on
```

On Debian and Ubuntu and other similar distributions, the <a undefined>update-rc.d</a> command can be used:

```sql
update-rc.d mysql defaults
```

### Starting the MariaDB Server Process

```sql
service mysql start
```

### Stopping the MariaDB Server Process

```sql
service mysql stop
```

### Restarting the MariaDB Server Process

```sql
service mysql restart
```

### Checking the Status of the MariaDB Server Process

```sql
service mysql status
```

## Manually Installing mysql.server with SysVinit

If you install MariaDB from [source](/mariadb-administration/getting-installing-and-upgrading-mariadb/compiling-mariadb-from-source/) or from a [binary tarball](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-binary-tarballs/) that does not install [mysql.server](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqlserver/)
automatically, and if you are on a system that uses [sysVinit](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/sysvinit/), then you can manually install `mysql.server` with [sysVinit](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/sysvinit/). See [mysql.server: Manually Installing with SysVinit](/kb/en/mysqlserver/#manually-installing-with-sysvinit) for more information.

## SysVinit and Galera Cluster

### Bootstrapping a New Cluster

When using [Galera Cluster](/kb/en/galera/) with sysVinit, the first node in a cluster has to be started with `service mysql bootstrap`. See [Getting Started with MariaDB Galera Cluster: Bootstrapping a New Cluster](/kb/en/getting-started-with-mariadb-galera-cluster/#bootstrapping-a-new-cluster) for more information.