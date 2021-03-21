# Using InnoDB Instead of XtraDB

By default [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and earlier releases come compiled with [XtraDB](/columns-storage-engines-and-plugins/storage-engines/innodb/about-xtradb) as the default InnoDB replacement. From [MariaDB 10.2](/kb/en/what-is-mariadb-102/), InnoDB is the default.

##### MariaDB starting with [5.5](/kb/en/what-is-mariadb-55/)

Starting from [MariaDB 5.5](/kb/en/what-is-mariadb-55/), all standard MariaDB distributions also includes
InnoDB as a plugin.

To use the InnoDB plugin instead of XtraDB you can add to your [my.cnf](/kb/en/mysqld-startup-options/) file:

```sql
[mysqld]
ignore_builtin_innodb
plugin_load=ha_innodb.so
# The following should not be needed if you are using a mariadb package:
plugin_dir=/usr/local/mysql/lib/mysql/plugin
```

##### MariaDB until [5.3](/kb/en/what-is-mariadb-53/)

For [MariaDB 5.3](/kb/en/what-is-mariadb-53/) and below, the name of the library is <code class="fixed" style="white-space:pre-wrap">ha_innodb_plugin.so</code>

The reasons you may want to use InnoDB instead of XtraDB are:

- You want to benchmark the difference between InnoDB/XtraDB
- You hit a bug in XtraDB
- You got a table space crash in XtraDB and recovery doesn't work. In some cases InnoDB may do a better job to recover data.

## See Also

- [Compiling with the InnoDB plugin from Oracle](/mariadb-administration/getting-installing-and-upgrading-mariadb/compiling-mariadb-from-source/compiling-with-the-innodb-plugin-from-oracle)