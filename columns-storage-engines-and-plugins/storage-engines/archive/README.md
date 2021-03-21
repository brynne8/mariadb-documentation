# Archive

The `ARCHIVE` storage engine is a storage engine that uses gzip to compress rows. It is mainly used for storing large amounts of data, without indexes, with only a very small footprint.

A table using the `ARCHIVE` storage engine is stored in two files on disk. There's a table definition file with an extension of .frm, and a data file with the extension .ARZ. At times during optimization, a .ARN file will appear.

New rows are inserted into a compression buffer and are flushed to disk when needed. SELECTs cause a flush. Sometimes, rows created by multi-row inserts are not visible until the statement is complete.

`ARCHIVE` allows a maximum of one key. The key must be on an [AUTO_INCREMENT](/columns-storage-engines-and-plugins/data-types/auto_increment/) column, and can be a `PRIMARY KEY` or a non-unique key. However, it has a limitation: it is not possible to insert a value which is lower than the next `AUTO_INCREMENT` value.

## Installing the Plugin

##### MariaDB starting with [10.1](/kb/en/what-is-mariadb-101/)

The `ARCHIVE` storage engine was installed by default until [MariaDB 10.0](/kb/en/what-is-mariadb-100/). In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and later, the storage engine's plugin will have to be installed.

Although the plugin's shared library is distributed with MariaDB by default, the plugin is not actually installed by MariaDB by default. There are two methods that can be used to install the plugin with MariaDB.

The first method can be used to install the plugin without restarting the server. You can install the plugin dynamically by executing [INSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-soname/) or [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin/). For example:

```sql
INSTALL SONAME 'ha_archive';
```

The second method can be used to tell the server to load the plugin when it starts up. The plugin can be installed this way by providing the <a undefined>--plugin-load</a> or the <a undefined>--plugin-load-add</a> options. This can be specified as a command-line argument to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) or it can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/). For example:

```sql
[mariadb]
...
plugin_load_add = ha_archive
```

## Uninstalling the Plugin

You can uninstall the plugin dynamically by executing [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname/) or [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin/). For example:

```sql
UNINSTALL SONAME 'ha_archive';
```

If you installed the plugin by providing the <a undefined>--plugin-load</a> or the <a undefined>--plugin-load-add</a> options in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/), then those options should be removed to prevent the plugin from being loaded the next time the server is restarted.

## Characteristics

- Supports [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert/) and [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/), but not [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete/), [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update/) or [REPLACE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/replace/).
- Data is compressed with zlib as it is inserted, making it very small.
- Data is slow the select, as it needs to be uncompressed, and, besides the [query cache](/replication/optimization-and-tuning/buffers-caches-and-threads/query-cache/), there is no cache.
- Supports AUTO_INCREMENT (since MariaDB/MySQL 5.1.6), which can be a unique or a non-unique index.
- Since MariaDB/MySQL 5.1.6, selects scan past BLOB columns unless they are specifically requested, making these queries much more efficient.
- Does not support [spatial](/kb/en/spatial/) data types.
- Does not support [transactions](/sql-statements-structure/sql-statements/transactions/).
- Does not support foreign keys.
- Does not support [virtual columns](/kb/en/virtual-columns/).
- No storage limit.
- Supports row locking.
- Supports [table discovery](/columns-storage-engines-and-plugins/storage-engines/storage-engines-storage-engine-development/table-discovery/), and the server can access ARCHIVE tables even if the corresponding `.frm` file is missing.
- [OPTIMIZE TABLE](/replication/optimization-and-tuning/optimizing-tables/optimize-table/) and [REPAIR TABLE](/sql-statements-structure/sql-statements/table-statements/repair-table/) can be used to compress the table in its entirety, resulting in slightly better compression.
- With MariaDB, it is possible to upgrade from the MySQL 5.0 format without having to dump the tables.
- [INSERT DELAYED](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-delayed/) is supported.
- Running many SELECTs during the insertions can deteriorate the compression, unless only multi-rows INSERTs and INSERT DELAYED are used.

- No items found.