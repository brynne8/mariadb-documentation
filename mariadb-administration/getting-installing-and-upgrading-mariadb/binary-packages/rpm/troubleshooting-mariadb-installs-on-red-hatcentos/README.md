# Troubleshooting MariaDB Installs on Red Hat/CentOS

The following article is about different issues people have encountered when installing MariaDB on Red Hat / CentOS.

It is highly recommended to [install with yum](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm/yum/) where possible.

In Red Hat / CentOS it is also possible to install a [RPM](/replication/optimization-and-tuning/query-optimizations/guiduuid-performance/mariadb/) or a [tar ball](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-binary-tarballs/). The RPM is the preferred version, except if you want to install many versions of MariaDB or install MariaDB in a non standard location.

### Replacing MySQL

If you removed an MySQL RPM to install MariaDB, note that the MySQL RPM on uninstall renames /etc/my.cnf to /etc/my.cnf.rpmsave.

After installing MariaDB you should do the following to restore your configuration options:

```sql
mv /etc/my.cnf.rpmsave /etc/my.cnf
```

### Unsupported configuration options

If you are using any of the following options in your /etc/my.cnf or other my.cnf file you should remove them.  This is also true for MySQL 5.1 or newer:

```sql
skip-bdb
```

## See also

- [Installing with yum (recommended)](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm/yum/)
- [Installing MariaDB RPM Files](/kb/en/installing-mariadb-rpm-files/)
- [Checking MariaDB RPM Package Signatures](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm/checking-mariadb-rpm-package-signatures/)