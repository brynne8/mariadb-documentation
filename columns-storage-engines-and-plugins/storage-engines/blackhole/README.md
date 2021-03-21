# BLACKHOLE

The `BLACKHOLE` storage engine accepts data but does not store it and always returns an empty result.

A table using the `BLACKHOLE` storage engine consists of a single .frm table format file, but no associated data or index files.

This storage engine can be useful, for example, if you want to run complex filtering rules on a slave without incurring any overhead on a master. The master can run a `BLACKHOLE` storage engine, with the data replicated to the slave for processing.

## Installing the Plugin

##### MariaDB starting with [10.1](/kb/en/what-is-mariadb-101/)

The `BLACKHOLE` storage engine was installed by default until [MariaDB 10.0](/kb/en/what-is-mariadb-100/). In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and later, the storage engine's plugin will have to be installed.

Although the plugin's shared library is distributed with MariaDB by default, the plugin is not actually installed by MariaDB by default. There are two methods that can be used to install the plugin with MariaDB.

The first method can be used to install the plugin without restarting the server. You can install the plugin dynamically by executing [INSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-soname/) or [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin/). For example:

```sql
INSTALL SONAME 'ha_blackhole';
```

The second method can be used to tell the server to load the plugin when it starts up. The plugin can be installed this way by providing the [--plugin-load](/kb/en/mysqld-options/#-plugin-load) or the [--plugin-load-add](/kb/en/mysqld-options/#-plugin-load-add) options. This can be specified as a command-line argument to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) or it can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/). For example:

```sql
[mariadb]
...
plugin_load_add = ha_blackhole
```

## Uninstalling the Plugin

You can uninstall the plugin dynamically by executing [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname/) or [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin/). For example:

```sql
UNINSTALL SONAME 'ha_blackhole';
```

If you installed the plugin by providing the [--plugin-load](/kb/en/mysqld-options/#-plugin-load) or the [--plugin-load-add](/kb/en/mysqld-options/#-plugin-load-add) options in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/), then those options should be removed to prevent the plugin from being loaded the next time the server is restarted.

## Using the BLACKHOLE Storage Engine

### Using with DML

[INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert/), [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update/), and [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete/) statements all work with the `BLACKHOLE` storage engine. However, no data changes are actually applied.

### Using with Replication

If the binary log is enabled, all SQL statements will be logged as usual, and replicated to any slave servers. However, since rows are not stored, it is important to use statement-based rather than the row or mixed format, as [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update/) and [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete/) statements are neither logged nor replicated. See [Binary Log Formats](/mariadb-administration/server-monitoring-logs/binary-log/binary-log-formats/).

### Using with Triggers

Some [triggers](/programming-customizing-mariadb/triggers-events/triggers/) work with the `BLACKHOLE` storage engine.

`BEFORE` [triggers](/programming-customizing-mariadb/triggers-events/triggers/) for [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert/) statements are still activated.

[Triggers](/programming-customizing-mariadb/triggers-events/triggers/) for [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update/) and [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete/) statements are <strong>not</strong> activated.

[Triggers](/programming-customizing-mariadb/triggers-events/triggers/) with the `FOR EACH ROW` clause do not apply, since the tables have no rows.

### Using with Foreign Keys

Foreign keys are not supported. If you convert an [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) table to `BLACKHOLE`, then the foreign keys will disappear. If you convert the same table back to InnoDB, then you will have to recreate them.

### Using with Virtual Columns

If you convert an [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) table which contains [virtual columns](/kb/en/virtual-columns/) to `BLACKHOLE`, then it produces an error.

### Using with AUTO_INCREMENT

Because a BLACKHOLE table does not store data, it will not maintain the [AUTO_INCREMENT](/columns-storage-engines-and-plugins/data-types/auto_increment/) value. If you are replicating to a table that can handle `AUTO_INCREMENT` columns, and are not explicitly setting the primary key auto-increment value in the [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert/) query, or using the [SET](/sql-statements-structure/sql-statements/administrative-sql-statements/set-commands/set/) [INSERT_ID](/kb/en/server-system-variables/#insert_id) statement, inserts will fail on the slave due to duplicate keys.

## Limits

The maximum key size is:

- 3500 bytes (&gt;= [MariaDB 10.1.48](/kb/en/mariadb-10148-release-notes/), [MariaDB 10.2.35](/kb/en/mariadb-10235-release-notes/), [MariaDB 10.3.26](/kb/en/mariadb-10326-release-notes/), [MariaDB 10.4.16](/kb/en/mariadb-10416-release-notes/) and [MariaDB 10.5.7](/kb/en/mariadb-1057-release-notes/))
- 1000 bytes (&lt;= [MariaDB 10.1.47](/kb/en/mariadb-10147-release-notes/), [MariaDB 10.2.34](/kb/en/mariadb-10234-release-notes/), [MariaDB 10.3.25](/kb/en/mariadb-10325-release-notes/), [MariaDB 10.4.15](/kb/en/mariadb-10415-release-notes/) and [MariaDB 10.5.6](/kb/en/mariadb-1056-release-notes/)).