# Installing the CONNECT Storage Engine

The `CONNECT` storage engine enables MariaDB to access external local or remote data (MED). This is done by defining tables based on different data types, in particular files in various formats, data extracted from other DBMS or products (such as Excel or MongoDB) via ODBC or JDBC, or data retrieved from the environment (for example DIR, WMI, and MAC tables)

This storage engine supports table partitioning, MariaDB virtual columns and permits defining special columns such as ROWID, FILEID, and SERVID.

The storage engine must be installed before it can be used.

## Installing the Plugin's Package

The `CONNECT` storage engine's shared library is included in MariaDB packages as the `ha_connect.so` or `ha_connect.so` shared library on systems where it can be built.

### Installing on Linux

The `CONNECT` storage engine is included in [binary tarballs](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-binary-tarballs) on Linux.

#### Installing with a Package Manager

The `CONNECT` storage engine can also be installed via a package manager on Linux. In order to do so, your system needs to be configured to install from one of the MariaDB repositories.

You can configure your package manager to install it from MariaDB Corporation's MariaDB Package Repository by using the [MariaDB Package Repository setup script](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/mariadb-package-repository-setup-and-usage).

You can also configure your package manager to install it from MariaDB Foundation's MariaDB Repository by using the [MariaDB Repository Configuration Tool](https://downloads.mariadb.org/mariadb/repositories/).

##### Installing with yum/dnf

On RHEL, CentOS, Fedora, and other similar Linux distributions, it is highly recommended to install the relevant [RPM package](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm) from MariaDB's
repository using [yum](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm/yum) or <a undefined>dnf</a>. Starting with RHEL 8 and Fedora 22, `yum` has been replaced by `dnf`, which is the next major version of `yum`. However, `yum` commands still work on many systems that use `dnf`. For example:

```sql
sudo yum install MariaDB-connect-engine
```

##### Installing with apt-get

On Debian, Ubuntu, and other similar Linux distributions, it is highly recommended to install the relevant [DEB package](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-deb-files) from MariaDB's
repository using <a undefined>apt-get</a>. For example:

```sql
sudo apt-get install mariadb-plugin-connect-engine
```

##### Installing with zypper

On SLES, OpenSUSE, and other similar Linux distributions, it is highly recommended to install the relevant [RPM package](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm) from MariaDB's repository using [zypper](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm/installing-mariadb-with-zypper). For example:

```sql
sudo zypper install MariaDB-connect-engine
```

## Installing the Plugin

Once the shared library is in place, the plugin is not actually installed by MariaDB by default. There are two methods that can be used to install the plugin with MariaDB.

The first method can be used to install the plugin without restarting the server. You can install the plugin dynamically by executing [INSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-soname) or [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin). For example:

```sql
INSTALL SONAME 'ha_connect';
```

The second method can be used to tell the server to load the plugin when it starts up. The plugin can be installed this way by providing the <a undefined>--plugin-load</a> or the <a undefined>--plugin-load-add</a> options. This can be specified as a command-line argument to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options) or it can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files). For example:

```sql
[mariadb]
...
plugin_load_add = ha_connect
```

## Uninstalling the Plugin

You can uninstall the plugin dynamically by executing [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname) or [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin). For example:

```sql
UNINSTALL SONAME 'ha_connect';
```

If you installed the plugin by providing the <a undefined>--plugin-load</a> or the <a undefined>--plugin-load-add</a> options in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files), then those options should be removed to prevent the plugin from being loaded the next time the server is restarted.

## Installing Dependencies

The `CONNECT` storage engine has some external dependencies.

### Installing unixODBC

The `CONNECT` storage engine requires an ODBC library. On Unix-like systems, that usually means installing [unixODBC](http://www.unixodbc.org/). On some systems, this is installed as the `unixODBC` package. For example:

```sql
sudo yum install unixODBC
```

On other systems, this is installed as the `libodbc1` package. For example:

```sql
sudo apt-get install libodbc1
```

If you do not have the ODBC library installed, then you may get an error about a missing library when you attempt to install the plugin. For example:

```sql
INSTALL SONAME 'ha_connect';
ERROR 1126 (HY000): Can't open shared library '/home/ian/MariaDB_Downloads/10.1.17/lib/plugin/ha_connect.so' 
  (errno: 2, libodbc.so.1: cannot open shared object file: No such file or directory)
```

## See Also

- [PLUGIN OVERVIEW](/columns-storage-engines-and-plugins/plugins/plugin-overview)