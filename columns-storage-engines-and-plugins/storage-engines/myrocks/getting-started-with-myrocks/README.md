# Getting Started with MyRocks

##### MariaDB starting with [10.2.5](/kb/en/mariadb-1025-release-notes/)

The MyRocks storage engine was first released in [MariaDB 10.2.5](/kb/en/mariadb-1025-release-notes/).

MyRocks is a storage engine that adds the RocksDB database to MariaDB. RocksDB is an LSM database with a great compression ratio that is optimized for flash storage.

The storage engine must be installed before it can be used.

## Installing the Plugin's Package

The MyRocks storage engine's shared library is included in MariaDB packages as the `ha_rocksdb.so` or `ha_rocksdb.dll` shared library on systems where it can be built. The plugin was first included in [MariaDB 10.2.5](/kb/en/mariadb-1025-release-notes/).

### Installing on Linux

The MyRocks storage engine is included in [binary tarballs](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-binary-tarballs) on Linux.

#### Installing with a Package Manager

The MyRocks storage engine can also be installed via a package manager on Linux. In order to do so, your system needs to be configured to install from one of the MariaDB repositories.

You can configure your package manager to install it from MariaDB Corporation's MariaDB Package Repository by using the [MariaDB Package Repository setup script](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/mariadb-package-repository-setup-and-usage).

You can also configure your package manager to install it from MariaDB Foundation's MariaDB Repository by using the [MariaDB Repository Configuration Tool](https://downloads.mariadb.org/mariadb/repositories/).

##### Installing with yum/dnf

On RHEL, CentOS, Fedora, and other similar Linux distributions, it is highly recommended to install the relevant [RPM package](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm) from MariaDB's
repository using [yum](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm/yum) or <a undefined>dnf</a>. Starting with RHEL 8 and Fedora 22, `yum` has been replaced by `dnf`, which is the next major version of `yum`. However, `yum` commands still work on many systems that use `dnf`. For example:

```sql
sudo yum install MariaDB-rocksdb-engine
```

##### Installing with apt-get

On Debian, Ubuntu, and other similar Linux distributions, it is highly recommended to install the relevant [DEB package](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-deb-files) from MariaDB's
repository using <a undefined>apt-get</a>. For example:

```sql
sudo apt-get install mariadb-plugin-rocksdb
```

##### Installing with zypper

On SLES, OpenSUSE, and other similar Linux distributions, it is highly recommended to install the relevant [RPM package](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm) from MariaDB's repository using [zypper](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm/installing-mariadb-with-zypper). For example:

```sql
sudo zypper install MariaDB-rocksdb-engine
```

### Installing on Windows

The MyRocks storage engine is included in [MSI](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-msi-packages-on-windows) and [ZIP](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-windows-zip-packages) packages on Windows.

## Installing the Plugin

Once the shared library is in place, the plugin is not actually installed by MariaDB by default. There are two methods that can be used to install the plugin with MariaDB.

The first method can be used to install the plugin without restarting the server. You can install the plugin dynamically by executing [INSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-soname) or [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin). For example:

```sql
INSTALL SONAME 'ha_rocksdb';
```

The second method can be used to tell the server to load the plugin when it starts up. The plugin can be installed this way by providing the <a undefined>--plugin-load</a> or the <a undefined>--plugin-load-add</a> options. This can be specified as a command-line argument to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options) or it can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files). For example:

```sql
[mariadb]
...
plugin_load_add = ha_rocksdb
```

Note: When installed with a package manager, an option file that contains the <a undefined>--plugin-load-add</a> option may also be installed. The RPM package installs it as `/etc/my.cnf.d/rocksdb.cnf`, and the DEB package installs it as `/etc/mysql/mariadb.conf.d/rocksdb.cnf`

## Uninstalling the Plugin

You can uninstall the plugin dynamically by executing [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname) or [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin). For example:

```sql
UNINSTALL SONAME 'ha_rocksdb';
```

If you installed the plugin by providing the <a undefined>--plugin-load</a> or the <a undefined>--plugin-load-add</a> options in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files), then those options should be removed to prevent the plugin from being loaded the next time the server is restarted.

## Verifying the Installation

After installing MyRocks you will see RocksDB in the list of plugins:

```sql
SHOW PLUGINS;
+-------------------------------+----------+--------------------+---------------+---------+
| Name                          | Status   | Type               | Library       | License |
+-------------------------------+----------+--------------------+---------------+---------+
...
| ROCKSDB                       | ACTIVE   | STORAGE ENGINE     | ha_rocksdb.so | GPL     |
| ROCKSDB_CFSTATS               | ACTIVE   | INFORMATION SCHEMA | ha_rocksdb.so | GPL     |
| ROCKSDB_DBSTATS               | ACTIVE   | INFORMATION SCHEMA | ha_rocksdb.so | GPL     |
| ROCKSDB_PERF_CONTEXT          | ACTIVE   | INFORMATION SCHEMA | ha_rocksdb.so | GPL     |
| ROCKSDB_PERF_CONTEXT_GLOBAL   | ACTIVE   | INFORMATION SCHEMA | ha_rocksdb.so | GPL     |
| ROCKSDB_CF_OPTIONS            | ACTIVE   | INFORMATION SCHEMA | ha_rocksdb.so | GPL     |
| ROCKSDB_COMPACTION_STATS      | ACTIVE   | INFORMATION SCHEMA | ha_rocksdb.so | GPL     |
| ROCKSDB_GLOBAL_INFO           | ACTIVE   | INFORMATION SCHEMA | ha_rocksdb.so | GPL     |
| ROCKSDB_DDL                   | ACTIVE   | INFORMATION SCHEMA | ha_rocksdb.so | GPL     |
| ROCKSDB_INDEX_FILE_MAP        | ACTIVE   | INFORMATION SCHEMA | ha_rocksdb.so | GPL     |
| ROCKSDB_LOCKS                 | ACTIVE   | INFORMATION SCHEMA | ha_rocksdb.so | GPL     |
| ROCKSDB_TRX                   | ACTIVE   | INFORMATION SCHEMA | ha_rocksdb.so | GPL     |
...
+-------------------------------+----------+--------------------+---------------+---------+
```

## Compression

Supported compression types are listed in the [rocksdb_supported_compression_types](/kb/en/myrocks-system-variables/#rocksdb_supported_compression_types) variable. For example:

```sql
SHOW VARIABLES LIKE 'rocksdb_supported_compression_types';
+-------------------------------------+-------------+
| Variable_name                       | Value       |
+-------------------------------------+-------------+
| rocksdb_supported_compression_types | Snappy,Zlib |
+-------------------------------------+-------------+
```

See [MyRocks and Data Compression](/columns-storage-engines-and-plugins/storage-engines/myrocks/myrocks-and-data-compression) for more.

## System and Status Variables

All MyRocks [system variables](/columns-storage-engines-and-plugins/storage-engines/myrocks/myrocks-system-variables) and [status variables](/columns-storage-engines-and-plugins/storage-engines/myrocks/myrocks-status-variables) are prefaced with "rocksdb", so you can query them with, for example:

```sql
SHOW VARIABLES LIKE 'rocksdb%';
SHOW STATUS LIKE 'rocksdb%';
```