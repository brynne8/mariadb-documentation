# mysqld Options

##### MariaDB starting with [10.4.6](/kb/en/mariadb-1046-release-notes/)

From [MariaDB 10.4.6](/kb/en/mariadb-1046-release-notes/), `mariadbd` is a symlink to `mysqld`.

##### MariaDB starting with [10.5.2](/kb/en/mariadb-1052-release-notes/)

From [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), `mariadbd` is the name of the binary, with `mysqld` a symlink .

This page lists all of the options for `mysqld` / `mariadbd`, ordered by topic. For a full alphabetical list of all mysqld options, as well as server and status variables, see [Full list of MariaDB options, system and status variables](/mariadb-administration/variables-and-modes/full-list-of-mariadb-options-system-and-status-variables/).

In many cases, the entry here is a summary, and links to the full description.

By convention, [server variables](/replication/optimization-and-tuning/system-variables/server-system-variables/) have usually been specified with an underscore in the configuration files, and a dash on the command line. You can however specify underscores as dashes - they are interchangeable.

See [mysqld startup options](/kb/en/mysqld-startup-options/) for which files and groups mysqld reads for it's default options.

## Option Prefixes

#### `--autoset-*`

- <strong>Description:</strong> Sets the option value automatically. Only supported for certain options. Available in [MariaDB 10.1.7](/kb/en/mariadb-1017-release-notes/) and later.

#### `--disable-*`

- <strong>Description:</strong> For all boolean options, disables the setting (equivalent to setting it to `0`). Same as `--skip`.

#### `--enable-*`

- <strong>Description:</strong> For all boolean options, enables the setting (equivalent to setting it to `1`).

#### `--loose-*`

- <strong>Description:</strong> Don't produce an error if the option doesn't exist.

#### `--maximum-*`

- <strong>Description:</strong> Sets the maximum value for the option.

#### `--skip-*`

- <strong>Description:</strong> For all boolean options, disables the setting (equivalent to setting it to `0`). Same as `--disable`.

## Option File Options

#### `--defaults-extra-file`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--defaults-extra-file=name</code>
- <strong>Description:</strong> Read this extra option file after all other option files are read.
<ul start="1"><li>See [Configuring MariaDB with Option Files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/).
</li></ul>

---

#### `--defaults-file`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--defaults-file=name</code>
- <strong>Description:</strong> Only read options from the given option file.
<ul start="1"><li>See [Configuring MariaDB with Option Files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/).
</li></ul>

---

#### `--defaults-group-suffix`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--defaults-group-suffix=name</code>
- <strong>Description:</strong> In addition to the default option groups, also read option groups with the given suffix.
<ul start="1"><li>See [Configuring MariaDB with Option Files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/).
</li></ul>

---

#### `--no-defaults`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--no-defaults</code>
- <strong>Description:</strong> Don't read options from any option file.
<ul start="1"><li>See [Configuring MariaDB with Option Files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/).
</li></ul>

---

#### `--print-defaults`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--print-defaults</code>
- <strong>Description:</strong> Read options from option files, print all option values, and then exit the program.
<ul start="1"><li>See [Configuring MariaDB with Option Files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/).
</li></ul>

---

## Compatibility Options

The following options have been added to MariaDB to make it more compliant with
other MariaDB and MySQL versions:

#### `-a, --ansi`

- <strong>Description:</strong> Use ANSI SQL syntax instead of MySQL syntax. This mode will also set [transaction isolation level](/kb/en/set-transaction-isolation-level/#isolation-levels) [serializable](/kb/en/set-transaction-isolation-level/#serializable).

---

#### `--new`

- <strong>Description:</strong> Use new functionality that will exist in next version of MariaDB. This function exists to make it easier to prepare for an upgrade. For version 5.1 this functions enables the LIST and RANGE partitions functions for ndbcluster.

---

#### `--old-style-user-limits`

- <strong>Description:</strong> Enable old-style user limits (before MySQL 5.0.3, user resources were counted per each user+host vs. per account).

---

#### `--safe-mode`

- <strong>Description:</strong> Disable some potential unsafe optimizations. For 5.2, [INSERT DELAYED](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-delayed/) is disabled, [myisam_recover_options](/kb/en/myisam-system-variables/#myisam_recover_options) is set to DEFAULT (automatically recover crashed MyISAM files) and the [query cache](/replication/optimization-and-tuning/buffers-caches-and-threads/query-cache/) is disabled. For [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/) tables, disable bulk insert optimization to enable one to use [aria_read_log](/clients-utilities/aria-clients-and-utilities/aria_read_log/) to recover tables even if tables are deleted (good for testing recovery).

---

#### `--skip-new`

- <strong>Description:</strong>  Disables [--new](#-new) in 5.2. In 5.1 used to disable some new potentially unsafe functions.

---

### Compatibility Options and System Variables

- [--old](/kb/en/server-system-variables/#old)
- [--old-alter-table](/kb/en/server-system-variables/#old_alter_table)
- [--old-mode](/kb/en/server-system-variables/#old_mode)
- [--old-passwords](/kb/en/server-system-variables/#old_passwords)
- [--show-old-temporals](/kb/en/server-system-variables/#show_old_temporals)

## Locale Options

#### `--character-set-client-handshake`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--character-set-client-handshake</code>
- <strong>Description:</strong> Don't ignore client side character set value sent during handshake.

---

#### `--default-character-set`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--default-character-set=name</code>
- <strong>Description:</strong> Still available as an option for setting the default character set for clients and their connections, it was deprecated and removed in [MariaDB 10.2](/kb/en/what-is-mariadb-102/) as a server option. Use [character-set-server](/kb/en/server-system-variables/#character_set_server) instead.

---

#### `--language`

- <strong>Description:</strong> This option can be used to set the server's language for error messages. This option can be specified either as a language name or as the path to the directory storing the language's [error message file](/kb/en/error-log/#error-messages-file). See [Server Locales](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/server-locale/) for a list of supported locales and their associated languages.
<ul start="1"><li>This option is deprecated. Use the <a undefined>lc_messages</a> and <a undefined>lc_messages_dir</a> system variables instead.
</li><li>See [Setting the Language for Error Messages](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/setting-the-language-for-error-messages/) for more information.
</li></ul>

---

### Locale Options and System Variables

- [character-set-filesystem](/kb/en/server-system-variables/#character_set_filesystem)
- [character-set-client](/kb/en/server-system-variables/#character_set_client)
- [character-set-connection](/kb/en/server-system-variables/#character_set_connection)
- [character-set-database](/kb/en/server-system-variables/#character_set_database)
- [character-set-filesystem](/kb/en/server-system-variables/#character_set_filesystem)
- [character-set-results](/kb/en/server-system-variables/#character_set_results)
- [character-set-server](/kb/en/server-system-variables/#character_set_server)
- [character-set-system](/kb/en/server-system-variables/#character_set_system)
- [character-sets-dir](/kb/en/server-system-variables/#character_sets_dir)
- [collation-connection](/kb/en/server-system-variables/#collation_connection)
- [collation-database](/kb/en/server-system-variables/#collation_database)
- [collation-server](/kb/en/server-system-variables/#collation_server)
- [default-week-format](/kb/en/server-system-variables/#default_week_format)
- [default-time-zone](/kb/en/server-system-variables/#time_zone)
- [lc-messages](/kb/en/server-system-variables/#lc_messages)
- [lc-messages-dir](/kb/en/server-system-variables/#lc_messages_dir)
- [lc-time-names](/kb/en/server-system-variables/#lc_time_names)

## Windows Options

#### `--console`

- <strong>Description:</strong> Windows-only option that keeps the console window open and for writing log messages to stderr and stdout. If specified together with [--log-error](/kb/en/server-system-variables/#log_error), the last option will take precedence.

---

#### `--install`

- <strong>Description:</strong> Windows-only option that installs the `mysqld` process as a Windows service.
<ul start="1"><li>The Windows service created with this option [auto-starts](https://docs.microsoft.com/en-us/windows/desktop/Services/automatically-starting-services). If you want a service that is [started on demand](https://docs.microsoft.com/en-us/windows/desktop/Services/starting-services-on-demand), then use the <a undefined>--install-manual</a> option.
</li><li>This option takes a service name as an argument. If this option is provided without a service name, then the service name defaults to "MySQL".
</li><li>This option is deprecated and may be removed in a future version. See [MDEV-19358](https://jira.mariadb.org/browse/MDEV-19358) for more information.
</li></ul>

---

#### `--install-manual`

- <strong>Description:</strong> Windows-only option that installs the `mysqld` process as a Windows service.
<ul start="1"><li>The Windows service created with this option is [started on demand](https://docs.microsoft.com/en-us/windows/desktop/Services/starting-services-on-demand). If you want a service that [auto-starts](https://docs.microsoft.com/en-us/windows/desktop/Services/automatically-starting-services), use the <a undefined>--install</a> option. 
</li><li>This option takes a service name as an argument. If this option is provided without a service name, then the service name defaults to "MySQL".
</li><li>This option is deprecated and may be removed in a future version. See [MDEV-19358](https://jira.mariadb.org/browse/MDEV-19358) for more information.
</li></ul>

---

#### `--remove`

- <strong>Description:</strong> Windows-only option that removes the Windows service created by the <a undefined>--install</a> or <a undefined>--install-manual</a> options.
<ul start="1"><li>This option takes a service name as an argument. If this option is provided without a service name, then the service name defaults to "MySQL".
</li><li>This option is deprecated and may be removed in a future version. See [MDEV-19358](https://jira.mariadb.org/browse/MDEV-19358) for more information.
</li></ul>

---

#### `--slow-start-timeout`

- <strong>Description:</strong> Windows-only option that defines the maximum number of milliseconds that the service control manager should wait before trying to kill the Windows service during startup. Defaults to `15000`.

---

#### `--standalone`

- <strong>Description:</strong> Windows-only option that has no effect. Kept for compatibility reasons.

---

### Windows Options and System Variables

The following options and system variables are related to using MariaDB on Windows:

- <a undefined>--named-pipe</a>

## Replication and Binary Logging Options

The following options are related to [replication](/replication/) and the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/):

#### `--abort-slave-event-count`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--abort-slave-event-count=#</code>
- <strong>Description:</strong> Option used by mysql-test for debugging and testing of replication.

---

#### `--binlog-do-db`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--binlog-do-db=name</code>
- <strong>Description:</strong> This option allows you to configure a [replication master](/replication/) to write statements and transactions affecting databases that match a specified name into its [binary log](/mariadb-administration/server-monitoring-logs/binary-log/). Since the filtered statements or transactions will not be present in the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/), its replication slaves will not be able to replicate them.
<ul start="1"><li>This option will <strong>not</strong> work with cross-database updates with [statement-based logging](/kb/en/binary-log-formats/#statement-based-logging). See the [Statement-Based Logging](/kb/en/replication-filters/#statement-based-logging) section for more information.
</li><li>This option can <strong>not</strong> be set dynamically.
</li><li>When setting it on the command-line or in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/), the option does not accept a comma-separated list. If you would like to specify multiple filters, then you need to specify the option multiple times.
</li><li>See [Replication Filters](/replication/standard-replication/replication-filters/) for more information. 
</li></ul>

---

#### `--binlog-ignore-db`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--binlog-ignore-db=name</code>
- <strong>Description:</strong> This option allows you to configure a [replication master](/replication/) to <strong>not</strong> write statements and transactions affecting databases that match a specified name into its [binary log](/mariadb-administration/server-monitoring-logs/binary-log/). Since the filtered statements or transactions will not be present in the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/), its replication slaves will not be able to replicate them.
<ul start="1"><li>This option will <strong>not</strong> work with cross-database updates with [statement-based logging](/kb/en/binary-log-formats/#statement-based-logging). See the [Statement-Based Logging](/kb/en/replication-filters/#statement-based-logging) section for more information.
</li><li>This option can <strong>not</strong> be set dynamically.
</li><li>When setting it on the command-line or in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/), the option does not accept a comma-separated list. If you would like to specify multiple filters, then you need to specify the option multiple times.
</li><li>See [Replication Filters](/replication/standard-replication/replication-filters/) for more information. 
</li></ul>

---

#### `--binlog-row-event-max-size`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--binlog-row-event-max-size=#</code>
- <strong>Description:</strong> The maximum size of a row-based [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) event in bytes. Rows will be grouped into events smaller than this size if possible. The value has to be a multiple of 256.
- <strong>Default value</strong>
<ul start="1"><li>`8192` (&gt;= [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/))
</li><li>`1024` (&lt;= [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/))
</li></ul>

---

#### `--disconnect-slave-event-count`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--disconnect-slave-event-count=#</code>
- <strong>Description:</strong> Option used by mysql-test for debugging and testing of replication.

---

#### `--flashback`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--flashback</code>
- <strong>Description:</strong> Setup the server to use flashback. This enables the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) and sets `binlog_format=ROW`.
- <strong>Introduced:</strong> [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/)

---

#### `--init-rpl-role`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--init-rpl-role=name</code>
- <strong>Description:</strong> Set the replication role.

---

#### `--log-basename`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--log-basename=name</code>
- <strong>Description:</strong> Basename for all log files and the .pid file. This sets all log file names at once (in 'datadir') and is normally the only option you need for specifying log files. This is especially recommended to be set if you are using [replication](/replication/) as it ensures that your log file names are not dependent on your host name. Sets names for [log-bin](/kb/en/replication-and-binary-log-server-system-variables/#log_bin), [log-bin-index](/kb/en/replication-and-binary-log-server-system-variables/#log_bin_index), [relay-log](/kb/en/replication-and-binary-log-server-system-variables/#relay_log), [relay-log-index](/kb/en/replication-and-binary-log-server-system-variables/#relay_log_index), [general-log-file](/kb/en/server-system-variables/#general_log_file), <code class="fixed" style="white-space:pre-wrap">--log-slow-query-log-file</code>, <code class="fixed" style="white-space:pre-wrap">--log-error-file</code>, and [pid-file](/kb/en/server-system-variables/#pid_file).
- <strong>Introduced:</strong> [MariaDB 5.2](/kb/en/what-is-mariadb-52/)

---

#### `--log-bin-trust-routine-creators`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--log-bin-trust-routine-creators</code>
- <strong>Description:</strong> Deprecated,  use [log-bin-trust-function-creators](/kb/en/replication-and-binary-log-server-system-variables/#log_bin_trust_function_creators).

---

#### `--master-host`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--master-host=name</code>
- <strong>Description:</strong> Master hostname or IP address for replication. If not set, the slave thread will not be started. Note that the setting of master-host will be ignored if there exists a valid master.info file.

---

#### `--master-info-file`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--master-info-file=name</code>
- <strong>Description:</strong> Name and location of the file where the `MASTER_LOG_FILE` and `MASTER_LOG_POS` options (i.e. the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) position on the master) and most other [CHANGE MASTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to/) options are written. The [slave's I/O thread](/kb/en/replication-threads/#slave-io-thread) keeps this [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) position updated as it downloads events.
<ul start="1"><li>See [CHANGE MASTER TO: Option Persistence](/kb/en/change-master-to/#option-persistence) for more information.
</li></ul>

---

#### `--master-password`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--master-password=name</code>
- <strong>Description:</strong> The password the slave thread will authenticate with when connecting to the master. If not set, an empty password is assumed. The value in master.info will take precedence if it can be read.

---

#### `--master-port`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--master-port=#</code>
- <strong>Description:</strong> The port the master is listening on. If not set, the compiled setting of MYSQL_PORT is assumed. If you have not tinkered with configure options, this should be 3306. The value in master.info will take precedence if it can be read.

---

#### `--master-retry-count`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--master-retry-count=#</code>
- <strong>Description:</strong> Number of times a slave will attempt to connect to a master before giving up. The retry interval is determined by the MASTER_CONNECT_RETRY option for the CHANGE MASTER statement. A value of 0 means the slave will not stop attempting to reconnect. Reconnects are triggered when a slave has timed out. See [slave_net_timeout](/kb/en/replication-and-binary-log-server-system-variables/#slave_net_timeout).
- <strong>Default Value:</strong> `86400`
- <strong>Range - 32 bit:</strong> `0 to 4294967295`
- <strong>Range - 64 bit:</strong> `0 to 18446744073709551615`

---

#### `--master-ssl`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--master-ssl</code>
- <strong>Description:</strong> Enable the slave to [connect to the master using TLS](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/secure-connections-overview/).

---

#### `--master-ssl-ca`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--master-ssl-ca[=name]</code>
- <strong>Description:</strong> Master TLS CA file. Only applies if you have enabled [master-ssl](#-master-ssl).

---

#### `--master-ssl-capath`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--master-ssl-capath[=name]</code>
- <strong>Description:</strong> Master TLS CA path. Only applies if you have enabled [master-ssl](#-master-ssl).

---

#### `--master-ssl-cert`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--master-ssl-cert[=name]</code>
- <strong>Description:</strong> Master TLS certificate file name. Only applies if you have enabled [master-ssl](#-master-ssl).

---

#### `--master-ssl-cipher`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--master-ssl-cipher[=name]</code>
- <strong>Description:</strong> Master TLS cipher. Only applies if you have enabled [master-ssl](#-master-ssl).

---

#### `--master-ssl-key`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--master-ssl-key[=name]</code>
- <strong>Description:</strong> Master TLS keyfile name. Only applies if you have enabled [master-ssl](#-master-ssl).

---

#### `--master-user`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--master-user=name</code>
- <strong>Description:</strong> The username the slave thread will use for authentication when connecting to the master. The user must have FILE privilege. If the master user is not set, user test is assumed. The value in master.info will take precedence if it can be read.

---

#### `--max-binlog-dump-events`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--max-binlog-dump-events=#</code>
- <strong>Description:</strong> Option used by mysql-test for debugging and testing of replication.

---

#### `--replicate-rewrite-db`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--replicate-rewrite-db=master_database-&gt;slave_database</code>
- <strong>Description:</strong> This option allows you to configure a [replication slave](/replication/) to rewrite database names. It uses the format `master_database-&gt;slave_database`. If a slave encounters a [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) event in which the default database (i.e. the one selected by the [USE](/sql-statements-structure/sql-statements/administrative-sql-statements/use/) statement) is `master_database`, then the slave will apply the event in `slave_database` instead.
<ul start="1"><li>This option will <strong>not</strong> work with cross-database updates with [statement-based logging](/kb/en/binary-log-formats/#statement-based-logging). See the [Statement-Based Logging](/kb/en/replication-filters/#statement-based-logging) section for more information.
</li><li>This option only affects statements that involve tables. This option does not affect statements involving the database itself, such as [CREATE DATABASE](/sql-statements-structure/sql-statements/data-definition/create/create-database/), [ALTER DATABASE](/sql-statements-structure/sql-statements/data-definition/alter/alter-database/), and [DROP DATABASE](/sql-statements-structure/sql-statements/data-definition/drop/drop-database/). 
</li><li>This option can <strong>not</strong> be set dynamically.
</li><li>When setting it on the command-line or in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/), the option does not accept a comma-separated list. If you would like to specify multiple filters, then you need to specify the option multiple times.
</li><li>See [Replication Filters](/replication/standard-replication/replication-filters/) for more information. 
</li></ul>

---

#### `--replicate-same-server-id`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--replicate-same-server-id</code>
- <strong>Description:</strong> In replication, if set to 1, do not skip events having our server id. Default value is 0 (to break infinite loops in circular replication). Can't be set to 1 if [log-slave-updates](/kb/en/replication-and-binary-log-server-system-variables/#log_slave_updates) is used.

---

#### `--sporadic-binlog-dump-fail`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--sporadic-binlog-dump-fail</code>
- <strong>Description:</strong> Option used by mysql-test for debugging and testing of replication.

---

#### `--sysdate-is-now`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--sysdate-is-now</code>
- <strong>Description:</strong> Non-default option to alias [SYSDATE()](/built-in-functions/date-time-functions/sysdate/) to [NOW()](/built-in-functions/date-time-functions/now/) to make it safe for [replication](/replication/). Since 5.0, SYSDATE() has returned a `dynamic' value different for different invocations, even within the same statement.

---

### Replication and Binary Logging Options and System Variables

The following options and system variables are related to [replication](/replication/) and the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/):

- [auto-increment-increment](/kb/en/replication-and-binary-log-server-system-variables/#auto_increment_increment)
- [auto-increment-offset](/kb/en/replication-and-binary-log-server-system-variables/#auto_increment_offset)
- [binlog-annotate-row-events](/kb/en/replication-and-binary-log-server-system-variables/#binlog_annotate_row_events)
- [binlog-cache-size](/kb/en/replication-and-binary-log-server-system-variables/#binlog_cache_size)
- [binlog-checksum](/kb/en/replication-and-binary-log-server-system-variables/#binlog_checksum)
- [binlog-commit-wait-count](/kb/en/replication-and-binary-log-server-system-variables/#binlog_commit_wait_count)
- [binlog-commit-wait-usec](/kb/en/replication-and-binary-log-server-system-variables/#binlog_commit_wait_usec)
- [binlog-direct-non-transactional-updates|](/kb/en/replication-and-binary-log-server-system-variables/#binlog_direct_non_transactional_updates)
- [binlog-file-cache-size](/kb/en/replication-and-binary-log-server-system-variables/#binlog_file_cache_size)
- [binlog-format](/kb/en/replication-and-binary-log-server-system-variables/#binlog_format)
- [binlog-optimize-thread-scheduling](/kb/en/replication-and-binary-log-server-system-variables/#binlog_optimize_thread_scheduling)
- [binlog-row-image](/kb/en/replication-and-binary-log-server-system-variables/#binlog_row_image)
- [binlog-row-metadata](/kb/en/replication-and-binary-log-server-system-variables/#binlog_row_metadata)
- [binlog-stmt-cache-size](/kb/en/replication-and-binary-log-server-system-variables/#binlog_stmt_cache_size)
- [default-master-connection](/kb/en/replication-and-binary-log-server-system-variables/#default_master_connection)
- [gtid-cleanup-batch-size](/kb/en/global-transaction-id/#gtid_cleanup_batch_size)
- [gtid-domain-id](/kb/en/global-transaction-id/#gtid_domain_id)
- [gtid-ignore-duplicates](/kb/en/global-transaction-id/#gtid_ignore_duplicates)
- [gtid-strict-mode](/kb/en/global-transaction-id/#gtid_strict_mode)
- [init-slave](/kb/en/replication-and-binary-log-server-system-variables/#init_slave)
- [log-bin](/kb/en/replication-and-binary-log-server-system-variables/#log_bin)
- [log-bin-compress](/kb/en/replication-and-binary-log-server-system-variables/#log_bin_compress)
- [log-bin-compress-min-len](/kb/en/replication-and-binary-log-server-system-variables/#log_bin_compress_min_len)
- [log-bin-index](/kb/en/replication-and-binary-log-server-system-variables/#log_bin_index)
- [log-bin-trust-function-creators](/kb/en/replication-and-binary-log-server-system-variables/#log_bin_trust_function_creators)
- [log-slave-updates](/kb/en/replication-and-binary-log-server-system-variables/#log_slave_updates)
- [master-verify-checksum](/kb/en/replication-and-binary-log-server-system-variables/#master_verify_checksum)
- [max-binlog-cache-size](/kb/en/replication-and-binary-log-server-system-variables/#max_binlog_cache_size)
- [max-binlog-size](/kb/en/replication-and-binary-log-server-system-variables/#max_binlog_size)
- [max-binlog-stmt-cache-size](/kb/en/replication-and-binary-log-server-system-variables/#max_binlog_stmt_cache_size)
- [max-relay-log-size](/kb/en/replication-and-binary-log-server-system-variables/#max_relay_log_size)
- [read-binlog-speed-limit](/kb/en/replication-and-binary-log-server-system-variables/#read_binlog_speed_limit)
- [relay-log](/kb/en/replication-and-binary-log-server-system-variables/#relay_log)
- [relay-log-index](/kb/en/replication-and-binary-log-server-system-variables/#relay_log_index)
- [relay-log-info-file](/kb/en/replication-and-binary-log-server-system-variables/#relay_log_info_file)
- [relay-log-purge](/kb/en/replication-and-binary-log-server-system-variables/#relay_log_purge)
- [relay-log-recovery](/kb/en/replication-and-binary-log-server-system-variables/#relay_log_recovery)
- [relay-log-space-limit](/kb/en/replication-and-binary-log-server-system-variables/#relay_log_space_limit)
- [replicate-annotate-row-events](/kb/en/replication-and-binary-log-server-system-variables/#replicate_annotate_row_events)
- [replicate-do-db](/kb/en/replication-and-binary-log-server-system-variables/#replicate_do_db)
- [replicate-do-table](/kb/en/replication-and-binary-log-server-system-variables/#replicate_do_table)
- [replicate-events-marked-for-skip](/kb/en/replication-and-binary-log-server-system-variables/#replicate_events_marked_for_skip)
- [replicate-ignore-db](/kb/en/replication-and-binary-log-server-system-variables/#replicate_ignore_db)
- [replicate-ignore-table](/kb/en/replication-and-binary-log-server-system-variables/#replicate_ignore_table)
- [replicate-wild-do-table](/kb/en/replication-and-binary-log-server-system-variables/#replicate_wild_do_table)
- [replicate-wild-ignore-table](/kb/en/replication-and-binary-log-server-system-variables/#replicate_wild_ignore_table)
- [report-host](/kb/en/replication-and-binary-log-server-system-variables/#report_host)
- [report-password](/kb/en/replication-and-binary-log-server-system-variables/#report_password)
- [report-port](/kb/en/replication-and-binary-log-server-system-variables/#report_port)
- [report-user](/kb/en/replication-and-binary-log-server-system-variables/#report_user)
- [rpl-recovery-rank](/kb/en/replication-and-binary-log-server-system-variables/#rpl_recovery_rank)
- [server-id](/kb/en/replication-and-binary-log-server-system-variables/#server_id)
- [slave-compressed-protocol](/kb/en/replication-and-binary-log-server-system-variables/#slave_compressed_protocol)
- [slave-ddl-exec-mode](/kb/en/replication-and-binary-log-server-system-variables/#slave_ddl_exec_mode)
- [slave-domain-parallel-threads](/kb/en/replication-and-binary-log-server-system-variables/#slave_domain_parallel_threads)
- [slave-exec-mode](/kb/en/replication-and-binary-log-server-system-variables/#slave_exec_mode)
- [slave-load-tmpdir](/kb/en/replication-and-binary-log-server-system-variables/#slave_load_tmpdir)
- [slave-max-allowed-packet](/kb/en/replication-and-binary-log-server-system-variables/#slave_max_allowed_packet)
- [slave-net-timeout](/kb/en/replication-and-binary-log-server-system-variables/#slave_net_timeout)
- [slave-parallel-max-queued](/kb/en/replication-and-binary-log-server-system-variables/#slave_parallel_max_queued)
- [slave-parallel-threads](/kb/en/replication-and-binary-log-server-system-variables/#slave_parallel_threads)
- [slave-run-triggers-for-rbr](/kb/en/replication-and-binary-log-server-system-variables/#slave_run_triggers_for_rbr)
- [slave-skip-errors](/kb/en/replication-and-binary-log-server-system-variables/#slave_skip_errors)
- [slave-sql-verify-checksum](/kb/en/replication-and-binary-log-server-system-variables/#slave_sql_verify_checksum)
- [slave-transaction-retries](/kb/en/replication-and-binary-log-server-system-variables/#slave_transaction_retries)
- [slave_transaction_retry_errors](/kb/en/replication-and-binary-log-server-system-variables/#slave_transaction_retry_errors)
- [slave_transaction_retry_interval](/kb/en/replication-and-binary-log-server-system-variables/#slave_transaction_retry_interval)
- [slave-type-conversions](/kb/en/replication-and-binary-log-server-system-variables/#slave_type_conversions)
- [sync-binlog](/kb/en/replication-and-binary-log-server-system-variables/#sync_binlog)
- [sync-master-info](/kb/en/replication-and-binary-log-server-system-variables/#sync_master_info)
- [sync-relay-log](/kb/en/replication-and-binary-log-server-system-variables/#sync_relay_log)
- [sync-relay-log-info](/kb/en/replication-and-binary-log-server-system-variables/#sync_relay_log_info)

### Semisynchronous Replication Options and System Variables

The options and system variables related to [Semisynchronous Replication](/replication/standard-replication/semisynchronous-replication/) are described [here](/kb/en/semisynchronous-replication/#system-variables).

## Optimizer Options

#### `--record-buffer`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--record-buffer=#</code>
- <strong>Description:</strong> Old alias for [read_buffer_size](/kb/en/server-system-variables/#read_buffer_size).
- <strong>Removed:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/)

---

#### `--table-cache`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--table-open-cache=#</code>
- <strong>Description:</strong> Removed; use [--table-open-cache](/kb/en/server-system-variables/#table_open_cache) instead.
- <strong>Removed:</strong> [MariaDB 5.1.3](/kb/en/mariadb-513-release-notes/)

---

### Optimizer Options and System Variables

- [alter-algorithm](/kb/en/server-system-variables/#alter_algorithm)
- [analyze-sample-percentage](/kb/en/server-system-variables/#analyze_sample_percentage)
- [big-tables](/kb/en/server-system-variables/#big_tables)
- [bulk-insert-buffer-size](/kb/en/server-system-variables/#bulk_insert_buffer_size)
- [expensive-subquery-limit](/kb/en/server-system-variables/#expensive_subquery_limit)
- [join-buffer-size](/kb/en/server-system-variables/#join_buffer_size)
- [join-buffer-space-limit](/kb/en/server-system-variables/#join_buffer_space_limit)
- [join-cache-level](/kb/en/server-system-variables/#join_cache_level)
- [max-heap-table-size](/kb/en/server-system-variables/#max_heap_table_size)
- [max-join-size](/kb/en/server-system-variables/#max_join_size)
- [max-seeks-for-key](/kb/en/server-system-variables/#max_seeks_for_key)
- [max-sort-length](/kb/en/server-system-variables/#max_sort_length)
- [mrr-buffer-size](/kb/en/server-system-variables/#mrr_buffer_size)
- [optimizer-max-sel-arg-weight](/kb/en/server-system-variables/#optimizer_max_sel_arg_weight)
- [optimizer-prune-level](/kb/en/server-system-variables/#optimizer_prune_level)
- [optimizer-search-depth](/kb/en/server-system-variables/#optimizer_search_depth)
- [optimizer-selectivity-sampling-limit](/kb/en/server-system-variables/#optimizer_selectivity_sampling_limit)
- [optimizer-switch](/kb/en/server-system-variables/#optimizer_switch)
- [optimizer-trace](/kb/en/server-system-variables/#optimizer_trace)
- [optimizer-trace-max-mem-size](/kb/en/server-system-variables/#optimizer_trace_max_mem_size)
- [optimizer-use-condition-selectivity](/kb/en/server-system-variables/#optimizer_use_condition_selectivity)
- [query-alloc-block-size](/kb/en/server-system-variables/#query_alloc_block_size)
- [query-prealloc-size](/kb/en/server-system-variables/#query_prealloc_size)
- [range-alloc-block-size](/kb/en/server-system-variables/#range_alloc_block_size)
- [read-buffer-size](/kb/en/server-system-variables/#read_buffer_size)
- [rowid-merge-buff-size](/kb/en/server-system-variables/#rowid_merge_buff_size)
- [table-definition-cache](/kb/en/server-system-variables/#table_definition_cache)
- [table-open-cache](/kb/en/server-system-variables/#table_open_cache)
- [table-open-cache-instances](/kb/en/server-system-variables/#table_open_cache_instances)
- [tmp-disk-table-size](/kb/en/server-system-variables/#tmp_disk_table_size)
- [tmp-memory-table-size](/kb/en/server-system-variables/#tmp_memory_table_size)
- [tmp-table-size](/kb/en/server-system-variables/#tmp_table_size)
- [use-stat-tables](/kb/en/server-system-variables/#use_stat_tables)

## Storage Engine Options

#### `--skip-bdb`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">----skip-bdb</code>
- <strong>Description:</strong> Deprecated option; Exists only for compatibility with very old my.cnf files.
- <strong>Removed:</strong> [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/)

---

### MyISAM Storage Engine Options

The options related to the [MyISAM](/columns-storage-engines-and-plugins/storage-engines/myisam-storage-engine/) storage engine are described below.

#### `--external-locking`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--external-locking</code>
- <strong>Description:</strong> Use system (external) locking (disabled by default).  With this option enabled you can run [myisamchk](/clients-utilities/myisam-clients-and-utilities/myisamchk/) to test (not repair) tables while the server is running. Disable with [--skip-external-locking](/kb/en/server-system-variables/#skip_external_locking).

---

#### `--log-isam`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--log-isam[=file_name]</code>
- <strong>Description:</strong> Enable the [MyISAM log](/mariadb-administration/server-monitoring-logs/myisam-log/), which logs all MyISAM changes to file. If no filename is provided, the default, <em>myisam.log</em> is used.

---

#### MyISAM Storage Engine Options and System Variables

Some options and system variables related to the [MyISAM](/columns-storage-engines-and-plugins/storage-engines/myisam-storage-engine/) storage engine can be found [here](/columns-storage-engines-and-plugins/storage-engines/myisam-storage-engine/myisam-system-variables/). Direct links to many of them can be found below.

- [concurrent-insert](/kb/en/server-system-variables/#concurrent_insert)
- [delayed-insert-limit](/kb/en/server-system-variables/#delayed_insert_limit)
- [delayed-insert-timeout](/kb/en/server-system-variables/#delayed_insert_timeout)
- [delayed-queue-size](/kb/en/server-system-variables/#delayed_queue_size)
- [keep-files-on-create](/kb/en/server-system-variables/#keep_files_on_create)
- [key-buffer-size](/kb/en/myisam-system-variables/#key_buffer_size)
- [key-cache-age-threshold](/kb/en/myisam-system-variables/#key_cache_age_threshold)
- [key-cache-block-size](/kb/en/myisam-system-variables/#key_cache_block_size)
- [key-cache-division-limit](/kb/en/myisam-system-variables/#key_cache_division_limit)
- [key-cache-file-hash-size](/kb/en/myisam-system-variables/#key_cache_file_hash_size)
- [key-cache-segments](/kb/en/myisam-system-variables/#key_cache_segments)
- [myisam-block-size](/kb/en/myisam-server-system-variables/#myisam_block_size)
- [myisam-data-pointer-size](/kb/en/myisam-server-system-variables/#myisam_data_pointer_size)
- [myisam-max-sort-file-size](/kb/en/myisam-server-system-variables/#myisam_max_sort_file_size)
- [myisam-mmap-size](/kb/en/myisam-server-system-variables/#myisam_mmap_size)
- [myisam-recover-options](/kb/en/myisam-server-system-variables/#myisam_recover_options)
- [myisam-repair-threads](/kb/en/myisam-server-system-variables/#myisam_repair_threads)
- [myisam-sort-buffer-size](/kb/en/myisam-server-system-variables/#myisam_sort_buffer_size)
- [myisam-stats-method](/kb/en/myisam-server-system-variables/#myisam_stats_method)
- [myisam-use-mmap](/kb/en/myisam-server-system-variables/#myisam_use_mmap)

### InnoDB Storage Engine Options

The options related to the [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) storage engine are described below.

#### `--innodb`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb=value</code>, <code class="fixed" style="white-space:pre-wrap">--skip-innodb</code>
- <strong>Description:</strong> This variable controls whether or not to load the InnoDB storage engine. Possible values are `ON`, `OFF`, `FORCE` or `FORCE_PLUS_PERMANENT` (from [MariaDB 5.5](/kb/en/what-is-mariadb-55/)). If set to `OFF` (the same as --skip-innodb), since InnoDB is the default storage engine, the server will not start unless another storage engine has been chosen with [--default-storage-engine](/kb/en/server-system-variables/#default_storage_engine). `FORCE` means that the storage engine must be successfully loaded, or else the server won't start. `FORCE_PLUS_PERMANENT` enables the plugin, but if plugin cannot initialize, the server will not start. In addition, the plugin cannot be uninstalled while the server is running.

---

#### `--innodb-cmp`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-cmp</code>
- <strong>Description:</strong>
- <strong>Default:</strong> `ON`

---

#### `--innodb-cmp-reset`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-cmp-reset</code>
- <strong>Description:</strong>
- <strong>Default:</strong> `ON`

---

#### `--innodb-cmpmem`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-cmpmem</code>
- <strong>Description:</strong>
- <strong>Default:</strong> `ON`

---

#### `--innodb-cmpmem-reset`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-cmpmem-reset</code>
- <strong>Description:</strong>
- <strong>Default:</strong> `ON`

---

#### `--innodb-file-io-threads`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-file-io-threads</code>
- <strong>Description:</strong>
- <strong>Default:</strong> `4`
- <strong>Removed:</strong> [MariaDB 10.3.0](/kb/en/mariadb-1030-release-notes/)

---

#### `--innodb-index-stats`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-index-stats</code>
- <strong>Description:</strong>
- <strong>Default:</strong> `ON`
- <strong>Removed:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `--innodb-lock-waits`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-lock-waits</code>
- <strong>Description:</strong>
- <strong>Default:</strong> `ON`

---

#### `--innodb-locks`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-locks</code>
- <strong>Description:</strong>
- <strong>Default:</strong> `ON`

---

#### `--innodb-rseg`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-rseg</code>
- <strong>Description:</strong>
- <strong>Default:</strong> `ON`
- <strong>Removed:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `--innodb-status-file`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-status-file</code>
- <strong>Description:</strong>
- <strong>Default:</strong> `FALSE`

---

#### `--innodb-sys-indexes`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-sys-indexes</code>
- <strong>Description:</strong>
- <strong>Default:</strong> `ON`

---

#### `--innodb-sys-stats`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-sys-stats</code>
- <strong>Description:</strong>
- <strong>Default:</strong> `ON`
- <strong>Removed:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `--innodb-sys-tables`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-sys-tables</code>
- <strong>Description:</strong>
- <strong>Default:</strong> `ON`

---

#### `--innodb-table-stats`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-table-stats</code>
- <strong>Description:</strong>
- <strong>Default:</strong> `ON`
- <strong>Removed:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `--innodb-trx`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--innodb-trx</code>
- <strong>Description:</strong>
- <strong>Default:</strong> `ON`

---

#### InnoDB Storage Engine Options and System Variables

Some options and system variables related to the [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) storage engine can be found [here](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-system-variables/). Direct links to many of them can be found below.

- [ignore-builtin-innodb](/kb/en/innodb-system-variables/#ignore_builtin_innodb)
- [innodb-adaptive-checkpoint](/kb/en/innodb-system-variables/#innodb_adaptive_checkpoint)
- [innodb-adaptive-flushing](/kb/en/innodb-system-variables/#innodb_adaptive_flushing)
- [innodb-adaptive-flushing-lwm](/kb/en/innodb-system-variables/#innodb_adaptive_flushing_lwm)
- [innodb-adaptive-flushing-method](/kb/en/innodb-system-variables/#innodb_adaptive_flushing_method)
- [innodb-adaptive-hash-index](/kb/en/innodb-system-variables/#innodb_adaptive_hash_index)
- [innodb-adaptive-hash-index-partitions](/kb/en/innodb-system-variables/#innodb_adaptive_hash_index_partitions)
- [innodb-adaptive-hash-index-parts](/kb/en/innodb-system-variables/#innodb_adaptive_hash_index_parts)
- [innodb-adaptive-max-sleep-delay](/kb/en/innodb-system-variables/#innodb_adaptive_max_sleep_delay)
- [innodb-additional-mem-pool-size](/kb/en/innodb-system-variables/#innodb_additional_mem_pool_size)
- [innodb-api-bk-commit-interval](/kb/en/innodb-system-variables/#innodb_api_bk_commit_interval)
- [innodb-api-disable-rowlock](/kb/en/innodb-system-variables/#innodb_api_disable_rowlock)
- [innodb-api-enable-binlog](/kb/en/innodb-system-variables/#innodb_api_enable_binlog)
- [innodb-api-enable-mdl](/kb/en/innodb-system-variables/#innodb_api_enable_mdl)
- [innodb-api-trx-level](/kb/en/innodb-system-variables/#innodb_api_trx_level)
- [innodb-auto-lru-dump](/kb/en/innodb-system-variables/#innodb_auto_lru_dump)
- [innodb-autoextend-increment](/kb/en/innodb-system-variables/#innodb_autoextend_increment)
- [innodb-autoinc-lock-mode](/kb/en/innodb-system-variables/#innodb_autoinc_lock_mode)
- [innodb-background-scrub-data-check-interval](/kb/en/innodb-system-variables/#innodb_background_scrub_data_check_interval)
- [innodb-background-scrub-data-compressed](/kb/en/innodb-system-variables/#innodb_background_scrub_data_compressed)
- [innodb-background-scrub-data-interval](/kb/en/innodb-system-variables/#innodb_background_scrub_data_interval)
- [innodb-background-scrub-data-uncompressed](/kb/en/innodb-system-variables/#innodb_background_scrub_data_uncompressed)
- [innodb-blocking-buffer-pool-restore](/kb/en/innodb-system-variables/#innodb_blocking_buffer_pool_restore)
- [innodb-buf-dump-status-frequency](/kb/en/innodb-system-variables/#innodb_buf_dump_status_frequency)
- [innodb-buffer-pool-chunk-size](/kb/en/innodb-system-variables/#innodb_buffer_pool_chunk_size)
- [innodb-buffer-pool-dump-at-shutdown](/kb/en/innodb-system-variables/#innodb_buffer_pool_dump_at_shutdown)
- [innodb-buffer-pool-dump-now](/kb/en/innodb-system-variables/#innodb_buffer_pool_dump_now)
- [innodb-buffer-pool-dump-pct](/kb/en/innodb-system-variables/#innodb_buffer_pool_dump_pct)
- [innodb-buffer-pool-evict](/kb/en/innodb-system-variables/#innodb_buffer_pool_evict)
- [innodb-buffer-pool-filename](/kb/en/innodb-system-variables/#innodb_buffer_pool_filename)
- [innodb-buffer-pool-instances](/kb/en/innodb-system-variables/#innodb_buffer_pool_instances)
- [innodb-buffer-pool-load-abort](/kb/en/innodb-system-variables/#innodb_buffer_pool_load_abort)
- [innodb-buffer-pool-load-at-startup](/kb/en/innodb-system-variables/#innodb_buffer_pool_load_at_startup)
- [innodb-buffer-pool-load-now](/kb/en/innodb-system-variables/#innodb_buffer_pool_load_now)
- [innodb-buffer-pool-load-pages-abort](/kb/en/innodb-system-variables/#innodb_buffer_pool_load_pages_abort)
- [innodb-buffer-pool-populate](/kb/en/innodb-system-variables/#innodb_buffer_pool_populate)
- [innodb-buffer-pool-restore-at-startup](/kb/en/innodb-system-variables/#innodb_buffer_pool_restore_at_startup)
- [innodb-buffer-pool-shm-checksum](/kb/en/innodb-system-variables/#innodb_buffer_pool_shm_checksum)
- [innodb-buffer-pool-shm-key](/kb/en/innodb-system-variables/#innodb_buffer_pool_shm_key)
- [innodb-buffer-pool-size](/kb/en/innodb-system-variables/#innodb_buffer_pool_size)
- [innodb-change-buffer-max-size](/kb/en/innodb-system-variables/#innodb_change_buffer_max_size)
- [innodb-change-buffering](/kb/en/innodb-system-variables/#innodb_change_buffering)
- [innodb-change-buffering-debug](/kb/en/innodb-system-variables/#innodb_change_buffering_debug)
- [innodb-checkpoint-age-target](/kb/en/innodb-system-variables/#innodb_checkpoint_age_target)
- [innodb-checksum-algorithm](/kb/en/innodb-system-variables/#innodb_checksum_algorithm)
- [innodb-checksums](/kb/en/innodb-system-variables/#innodb_checksums)
- [innodb-cleaner-lsn-age-factor](/kb/en/innodb-system-variables/#innodb_cleaner_lsn_age_factor)
- [innodb-cmp-per-index-enabled](/kb/en/innodb-system-variables/#innodb_cmp_per_index_enabled)
- [innodb-commit-concurrency](/kb/en/innodb-system-variables/#innodb_commit_concurrency)
- [innodb-compression-algorithm](/kb/en/innodb-system-variables/#innodb_compression_algorithm)
- [innodb-compression-failure-threshold-pct](/kb/en/innodb-system-variables/#innodb_compression_failure_threshold_pct)
- [innodb-compression-level](/kb/en/innodb-system-variables/#innodb_compression_level)
- [innodb-compression-pad-pct-max](/kb/en/innodb-system-variables/#innodb_compression_pad_pct_max)
- [innodb-concurrency-tickets](/kb/en/innodb-system-variables/#innodb_concurrency_tickets)
- [innodb-corrupt-table-action](/kb/en/innodb-system-variables/#innodb_corrupt_table_action)
- [innodb-data-file-path](/kb/en/innodb-system-variables/#innodb_data_file_path)
- [innodb-data-home-dir](/kb/en/innodb-system-variables/#innodb_data_home_dir)
- [innodb-deadlock-detect](/kb/en/innodb-system-variables/#innodb_deadlock_detect)
- [innodb-default-encryption-key-id](/kb/en/innodb-system-variables/#innodb_default_encryption_key_id)
- [innodb-default-page-encryption-key](/kb/en/innodb-system-variables/#innodb_default_page_encryption_key)
- [innodb-default-row-format](/kb/en/innodb-system-variables/#innodb_default_row_format)
- [innodb-defragment](/kb/en/innodb-system-variables/#innodb_defragment)
- [innodb-defragment-fill-factor](/kb/en/innodb-system-variables/#innodb_defragment_fill_factor)
- [innodb-defragment-fill-factor-n-recs](/kb/en/innodb-system-variables/#innodb_defragment_fill_factor_n_recs)
- [innodb-defragment-frequency](/kb/en/innodb-system-variables/#innodb_defragment_frequency)
- [innodb-defragment-n-pages](/kb/en/innodb-system-variables/#innodb_defragment_n_pages)
- [innodb-defragment-stats-accuracy](/kb/en/innodb-system-variables/#innodb_defragment_stats_accuracy)
- [innodb-dict-size-limit](/kb/en/innodb-system-variables/#innodb_dict_size_limit)
- [innodb_disable_sort_file_cache](/kb/en/innodb-system-variables/#innodb_disable_sort_file_cache)
- [innodb-doublewrite](/kb/en/innodb-system-variables/#innodb_doublewrite)
- [innodb-doublewrite-file](/kb/en/innodb-system-variables/#innodb_doublewrite_file)
- [innodb-empty-free-list-algorithm](/kb/en/innodb-system-variables/#innodb_empty_free_list_algorithm)
- [innodb-enable-unsafe-group-commit](/kb/en/innodb-system-variables/#innodb_enable_unsafe_group_commit)
- [innodb-encrypt-log](/kb/en/innodb-system-variables/#innodb_encrypt_log)
- [innodb-encrypt-tables](/kb/en/innodb-system-variables/#innodb_encrypt_tables)
- [innodb-encrypt-temporary-tables](/kb/en/innodb-system-variables/#innodb_encrypt_tables)
- [innodb-encryption-rotate-key-age](/kb/en/innodb-system-variables/#innodb_encryption_rotate_key_age)
- [innodb-encryption-rotation_iops](/kb/en/innodb-system-variables/#innodb_encryption_rotation_iops)
- [innodb-encryption-threads](/kb/en/innodb-system-variables/#innodb_encryption_threads)
- [innodb-extra-rsegments](/kb/en/innodb-system-variables/#innodb_extra_rsegments)
- [innodb-extra-undoslots](/kb/en/innodb-system-variables/#innodb_extra_undoslots)
- [innodb-fake-changes](/kb/en/innodb-system-variables/#innodb_fake_changes)
- [innodb-fast-checksum](/kb/en/innodb-system-variables/#innodb_fast_checksum)
- [innodb-fast-shutdown](/kb/en/innodb-system-variables/#innodb_fast_shutdown)
- [innodb-fatal-semaphore-wait-threshold](/kb/en/innodb-system-variables/#innodb_fatal_semaphore_wait_threshold)
- [innodb-file-format](/kb/en/innodb-system-variables/#innodb_file_format)
- [innodb-file-format-check](/kb/en/innodb-system-variables/#innodb_file_format_check)
- [innodb-file-format-max](/kb/en/innodb-system-variables/#innodb_file_format_max)
- [innodb-file-per-table](/kb/en/innodb-system-variables/#innodb_file_per_table)
- [innodb-fill-factor](/kb/en/innodb-system-variables/#innodb_fill_factor)
- [innodb-flush-log-at-trx-commit](/kb/en/innodb-system-variables/#innodb_flush_log_at_trx_commit)
- [innodb-flush-method](/kb/en/innodb-system-variables/#innodb_flush_method)
- [innodb-flush-neighbor-pages](/kb/en/innodb-system-variables/#innodb_flush_neighbor_pages)
- [innodb-flush-neighbors](/kb/en/innodb-system-variables/#innodb_flush_neighbors)
- [innodb-flush-sync](/kb/en/innodb-system-variables/#innodb_flush_sync)
- [innodb-flushing-avg-loops](/kb/en/innodb-system-variables/#innodb_flushing_avg_loops)
- [innodb-force-load-corrupted](/kb/en/innodb-system-variables/#innodb_force_load_corrupted)
- [innodb-force-primary-key](/kb/en/innodb-system-variables/#innodb_force_primary_key)
- [innodb-force-recovery](/kb/en/innodb-system-variables/#innodb_force_recovery)
- [innodb-foreground-preflush](/kb/en/innodb-system-variables/#innodb_foreground_preflush)
- [innodb-ft-aux-table](/kb/en/innodb-system-variables/#innodb_ft_aux_table)
- [innodb-ft-cache-size](/kb/en/innodb-system-variables/#innodb_ft_cache_size)
- [innodb-ft-enable-diag-print](/kb/en/innodb-system-variables/#innodb_ft_enable_diag_print)
- [innodb-ft-enable-stopword](/kb/en/innodb-system-variables/#innodb_ft_enable_stopword)
- [innodb-ft-max-token-size](/kb/en/innodb-system-variables/#innodb_ft_max_token_size)
- [innodb-ft-min-token-size](/kb/en/innodb-system-variables/#innodb_ft_min_token_size)
- [innodb-ft-num-word-optimize](/kb/en/innodb-system-variables/#innodb_ft_num_word_optimize)
- [innodb-ft-result-cache-limit](/kb/en/innodb-system-variables/#innodb_ft_result_cache_limit)
- [innodb-ft-server-stopword-table](/kb/en/innodb-system-variables/#innodb_ft_server_stopword_table)
- [innodb-ft-sort-pll-degree](/kb/en/innodb-system-variables/#innodb_ft_sort_pll_degree)
- [innodb-ft-total-cache-size](/kb/en/innodb-system-variables/#innodb_ft_total_cache_size)
- [innodb-ft-user-stopword-table](/kb/en/innodb-system-variables/#innodb_ft_user_stopword_table)
- [innodb-ibuf-accel-rate](/kb/en/innodb-system-variables/#innodb_ibuf_accel_rate)
- [innodb-ibuf-active-contract](/kb/en/innodb-system-variables/#innodb_ibuf_active_contract)
- [innodb-ibuf-max-size](/kb/en/innodb-system-variables/#innodb_ibuf_max_size)
- [innodb-idle-flush-pct](/kb/en/innodb-system-variables/#innodb_idle_flush_pct)
- [innodb-immediate-scrub-data-uncompressed](/kb/en/innodb-system-variables/#innodb_immediate_scrub_data_uncompressed)
- [innodb-import-table-from-xtrabackup](/kb/en/innodb-system-variables/#innodb_import_table_from_xtrabackup)
- [innodb-instant-alter-column-allowed](/kb/en/innodb-system-variables/#innodb_instant_alter_column_allowed)
- [innodb-instrument-semaphores](/kb/en/innodb-system-variables/#innodb_instrument_semaphores)
- [innodb-io-capacity](/kb/en/innodb-system-variables/#innodb_io_capacity)
- [innodb-io-capacity-max](/kb/en/innodb-system-variables/#innodb_io_capacity_max)
- [innodb-large-prefix](/kb/en/innodb-system-variables/#innodb_large_prefix)
- [innodb-lazy-drop-table](/kb/en/innodb-system-variables/#innodb_lazy_drop_table)
- [innodb-lock-schedule-algorithm](/kb/en/innodb-system-variables/#innodb_lock_schedule_algorithmt)
- [innodb-locking-fake-changes](/kb/en/innodb-system-variables/#innodb_locking_fake_changes)
- [innodb-locks-unsafe-for-binlog](/kb/en/innodb-system-variables/#innodb_locks_unsafe_for_binlog)
- [innodb-log-arch-dir](/kb/en/innodb-system-variables/#innodb_log_arch_dir)
- [innodb-log-arch-expire-sec](/kb/en/innodb-system-variables/#innodb_log_arch_expire_sec)
- [innodb-log-archive](/kb/en/innodb-system-variables/#innodb_log_archive)
- [innodb-log-block-size](/kb/en/innodb-system-variables/#innodb_log_block_size)
- [innodb-log-buffer-size](/kb/en/innodb-system-variables/#innodb_log_buffer_size)
- [innodb-log-checksum-algorithm](/kb/en/innodb-system-variables/#innodb_log_checksum_algorithm)
- [innodb-log-checksums](/kb/en/innodb-system-variables/#innodb_log_checksums)
- [innodb-log-compressed-pages](/kb/en/innodb-system-variables/#innodb_log_compressed_pages)
- [innodb-log-file-size](/kb/en/innodb-system-variables/#innodb_log_file_size)
- [innodb-log-files-in-group](/kb/en/innodb-system-variables/#innodb_log_files_in_group)
- [innodb-log-group-home-dir](/kb/en/innodb-system-variables/#innodb_log_group_home_dir)
- [innodb-log-optimize-ddl](/kb/en/innodb-system-variables/#innodb_log_optimize_ddl)
- [innodb-log-write-ahead-size](/kb/en/innodb-system-variables/#innodb_log_write_ahead_size)
- [innodb-lru-flush-size](/kb/en/innodb-system-variables/#innodb_lru_flush_size)
- [innodb-lru-scan-depth](/kb/en/innodb-system-variables/#innodb_lru_scan_depth)
- [innodb-max-bitmap-file-size](/kb/en/innodb-system-variables/#innodb_max_bitmap_file_size)
- [innodb-max-changed-pages](/kb/en/innodb-system-variables/#innodb_max_changed_pages)
- [innodb-max-dirty-pages-pct](/kb/en/innodb-system-variables/#innodb_max_dirty_pages_pct)
- [innodb-max-dirty-pages-pct-lwm](/kb/en/innodb-system-variables/#innodb_max_dirty_pages_pct_lwm)
- [innodb-max-purge-lag](/kb/en/innodb-system-variables/#innodb_max_purge_lag)
- [innodb-max-purge-lag-delay](/kb/en/innodb-system-variables/#innodb_max_purge_lag_delay)
- [innodb-max-purge-lag-wait](/kb/en/innodb-system-variables/#innodb_max_purge_lag_wait)
- [innodb-max-undo-log-size](/kb/en/innodb-system-variables/#innodb_max_undo_log_size)
- [innodb-merge-sort-block-size](/kb/en/innodb-system-variables/#innodb_merge_sort_block_size)
- [innodb-mirrored-log-groups](/kb/en/innodb-system-variables/#innodb_mirrored_log_groups)
- [innodb-monitor-disable](/kb/en/innodb-system-variables/#innodb_monitor_disable)
- [innodb-monitor-enable](/kb/en/innodb-system-variables/#innodb_monitor_enable)
- [innodb-monitor-reset](/kb/en/innodb-system-variables/#innodb_monitor_reset)
- [innodb-monitor-reset-all](/kb/en/innodb-system-variables/#innodb_monitor_reset_all)
- [innodb-mtflush-threads](/kb/en/innodb-system-variables/#innodb_mtflush_threads)
- [innodb-numa-interleave](/kb/en/innodb-system-variables/#innodb_numa_interleave)
- [innodb-old-blocks-pct](/kb/en/innodb-system-variables/#innodb_old_blocks_pct)
- [innodb-old-blocks-time](/kb/en/innodb-system-variables/#innodb_old_blocks_time)
- [innodb-online-alter-log-max-size](/kb/en/innodb-system-variables/#innodb_online_alter_log_max_size)
- [innodb-open-files](/kb/en/innodb-system-variables/#innodb_open_files)
- [innodb-optimize-fulltext-only](/kb/en/innodb-system-variables/#innodb_optimize_fulltext_only)
- [innodb-page-cleaners](/kb/en/innodb-system-variables/#innodb_page_cleaners)
- [innodb-page-size](/kb/en/innodb-system-variables/#innodb_page_size)
- [innodb-pass-corrupt-table](/kb/en/innodb-system-variables/#innodb_pass_corrupt_table)
- [innodb-prefix-index-cluster-optimization](/kb/en/innodb-system-variables/#innodb_prefix_index_cluster_optimization)
- [innodb-print-all-deadlocks](/kb/en/innodb-system-variables/#innodb_print_all_deadlocks)
- [innodb-purge-batch-size](/kb/en/innodb-system-variables/#innodb_purge_batch_size)
- [innodb-purge-rseg-truncate-frequency](/kb/en/innodb-system-variables/#innodb_purge_rseg_truncate_frequency)
- [innodb-purge-threads](/kb/en/innodb-system-variables/#innodb_purge_threads)
- [innodb-random-read-ahead](/kb/en/innodb-system-variables/#innodb_random_read_ahead)
- [innodb-read-ahead](/kb/en/innodb-system-variables/#innodb_read_ahead)
- [innodb-read-ahead-threshold](/kb/en/innodb-system-variables/#innodb_read_ahead_threshold)
- [innodb-read-io-threads](/kb/en/innodb-system-variables/#innodb_read_io_threads)
- [innodb-read-only](/kb/en/innodb-system-variables/#innodb_read_only)
- [innodb-recovery-update-relay-log](/kb/en/innodb-system-variables/#innodb_recovery_update_relay_log)
- [innodb-replication-delay](/kb/en/innodb-system-variables/#innodb_replication_delay)
- [innodb-rollback-on-timeout](/kb/en/innodb-system-variables/#innodb_rollback_on_timeout)
- [innodb-rollback-segments](/kb/en/innodb-system-variables/#innodb_rollback_segments)
- [innodb-safe-truncate](/kb/en/innodb-system-variables/#innodb_safe_truncate)
- [innodb-sched-priority-cleaner](/kb/en/innodb-system-variables/#innodb_sched_priority_cleaner)
- [innodb-scrub-log](/kb/en/innodb-system-variables/#innodb_scrub_log)
- [innodb-scrub-log-interval](/kb/en/innodb-system-variables/#innodb_scrub_log_interval)
- [innodb-scrub-log-speed](/kb/en/innodb-system-variables/#innodb_scrub_log_speed)
- [innodb-show-locks-held](/kb/en/innodb-system-variables/#innodb_show_locks_held)
- [innodb-show-verbose-locks](/kb/en/innodb-system-variables/#innodb_show_verbose_locks)
- [innodb-sort-buffer-size](/kb/en/innodb-system-variables/#innodb_sort_buffer_size)
- [innodb-spin-wait-delay](/kb/en/innodb-system-variables/#innodb_spin_wait_delay)
- [innodb-stats-auto-recalc](/kb/en/innodb-system-variables/#innodb_stats_auto_recalc)
- [innodb-stats-auto-update](/kb/en/innodb-system-variables/#innodb_stats_auto_update)
- [innodb-stats-include-delete-marked](/kb/en/innodb-system-variables/#innodb_stats_include_delete_marked)
- [innodb-stats-method](/kb/en/innodb-system-variables/#innodb_stats_method)
- [innodb-stats-modified-counter](/kb/en/innodb-system-variables/#innodb_stats_modified_counter)
- [innodb-stats-on-metadata](/kb/en/innodb-system-variables/#innodb_stats_on_metadata)
- [innodb-stats-persistent](/kb/en/innodb-system-variables/#innodb_stats_persistent)
- [innodb-stats-persistent-sample-pages](/kb/en/innodb-system-variables/#innodb_stats_persistent_sample_pages)
- [innodb-stats-sample-pages](/kb/en/innodb-system-variables/#innodb_stats_sample_pages)
- [innodb-stats-transient-sample-pages](/kb/en/innodb-system-variables/#innodb_stats_transient_sample_pages)
- [innodb-stats-traditional](/kb/en/innodb-system-variables/#innodb_stats_traditional)
- [innodb-stats-update-need-lock](/kb/en/innodb-system-variables/#innodb_stats_update_need_lock)
- [innodb-status-output](/kb/en/innodb-system-variables/#innodb_status_output)
- [innodb-status-output-locks](/kb/en/innodb-system-variables/#innodb_status_output_locks)
- [innodb-strict-mode](/kb/en/innodb-system-variables/#innodb_strict_mode)
- [innodb-support-xa](/kb/en/innodb-system-variables/#innodb_support_xa)
- [innodb-sync-array-size](/kb/en/innodb-system-variables/#innodb_sync_array_size)
- [innodb-sync-spin-loops](/kb/en/innodb-system-variables/#innodb_sync_spin_loops)
- [innodb-table-locks](/kb/en/innodb-system-variables/#innodb_table_locks)
- [innodb-temp-data-file-path](/kb/en/innodb-system-variables/#innodb_temp_data_file_path)
- [innodb-thread-concurrency](/kb/en/innodb-system-variables/#innodb_thread_concurrency)
- [innodb-thread-concurrency-timer-based](/kb/en/innodb-system-variables/#innodb_thread_concurrency_timer_based)
- [innodb-thread-sleep-delay](/kb/en/innodb-system-variables/#innodb_thread_sleep_delay)
- [innodb-tmpdir](/kb/en/innodb-system-variables/#innodb_tmpdir)
- [innodb-track-changed-pages](/kb/en/innodb-system-variables/#innodb_track_changed_pages)
- [innodb-track-redo-log-now](/kb/en/innodb-system-variables/#innodb_track_redo_log_now)
- [innodb-undo-directory](/kb/en/innodb-system-variables/#innodb_undo_directory)
- [innodb-undo-log-truncate](/kb/en/innodb-system-variables/#innodb_undo_log_truncate)
- [innodb-undo-logs](/kb/en/innodb-system-variables/#innodb_undo_logs)
- [innodb-undo-tablespaces](/kb/en/innodb-system-variables/#innodb_undo_tablespaces)
- [innodb-use-atomic-writes](/kb/en/innodb-system-variables/#innodb_use_atomic_writes)
- [innodb-use-fallocate](/kb/en/innodb-system-variables/#innodb_use_fallocate)
- [innodb-use-global-flush-log-at-trx-commit](/kb/en/innodb-system-variables/#innodb_use_global_flush_log_at_trx_commit)
- [innodb-use-mtflush](/kb/en/innodb-system-variables/#innodb_use_mtflush)
- [innodb-use-native_aio](/kb/en/innodb-system-variables/#innodb_use_native_aio)
- [innodb-use-purge-thread](/kb/en/innodb-system-variables/#innodb_use_purge_thread)
- [innodb-use-stacktrace](/kb/en/innodb-system-variables/#innodb_use_stacktrace)
- [innodb-use-sys-malloc](/kb/en/innodb-system-variables/#innodb_use_sys_malloc)
- [innodb-use-sys-stats-table](/kb/en/innodb-system-variables/#innodb_use_sys_stats_table)
- [innodb-use-trim](/kb/en/innodb-system-variables/#innodb_use_trim)
- [innodb-write-io-threads](/kb/en/innodb-system-variables/#innodb_write_io_threads)
- [skip-innodb](#-innodb)
- [skip-innodb-checksums](/kb/en/innodb-system-variables/#innodb_checksums)
- [skip-innodb-doublewrite](/kb/en/innodb-system-variables/#innodb_doublewrite)

### Aria Storage Engine Options

The options related to the [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/) storage engine are described below.

#### `--aria-log-dir-path`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--aria-log-dir-path=value</code>
- <strong>Description:</strong> Path to the directory where  transactional log should be stored
- <strong>Default:</strong> `SAME AS DATADIR`

---

#### Aria Storage Engine Options and System Variables

Some options and system variables related to the [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/) storage engine can be found [here](/columns-storage-engines-and-plugins/storage-engines/aria/aria-system-variables/). Direct links to many of them can be found below.

- [aria-block-size](/kb/en/aria-system-variables/#aria_block_size)
- [aria-checkpoint-interval](/kb/en/aria-system-variables/#aria_checkpoint_interval)
- [aria-checkpoint-log-activity](/kb/en/aria-system-variables/#aria_checkpoint_log_activity)
- [aria-encrypt-tables](/kb/en/aria-system-variables/#aria_encrypt_tables)
- [aria-force-start-after-recovery-failures](/kb/en/aria-system-variables/#aria_force_start_after_recovery_failures)
- [aria-group-commit](/kb/en/aria-system-variables/#aria_group_commit)
- [aria-group-commit-interval](/kb/en/aria-system-variables/#aria_group_commit_interval)
- [aria-log-file-size](/kb/en/aria-system-variables/#aria_log_file_size)
- [aria-log-purge-type](/kb/en/aria-system-variables/#aria_log_purge_type)
- [aria-max-sort-file-size](/kb/en/aria-system-variables/#aria_max_sort_file_size)
- [aria-page-checksum](/kb/en/aria-system-variables/#aria_page_checksum)
- [aria-pagecache-age-threshold](/kb/en/aria-system-variables/#aria_pagecache_age_threshold)
- [aria-pagecache-buffer-size](/kb/en/aria-system-variables/#aria_pagecache_buffer_size)
- [aria-pagecache-division-limit](/kb/en/aria-system-variables/#aria_pagecache_division_limit)
- [aria-pagecache-file-hash-size](/kb/en/aria-system-variables/#aria_pagecache_file_hash_size)
- [aria-recover](/kb/en/aria-system-variables/#aria_recover)
- [aria-recover-options](/kb/en/aria-system-variables/#aria_recover_options)
- [aria-repair-threads](/kb/en/aria-system-variables/#aria_repair_threads)
- [aria-sort-buffer-size](/kb/en/aria-system-variables/#aria_sort_buffer_size)
- [aria-stats-method](/kb/en/aria-system-variables/#aria_stats_method)
- [aria-sync-log-dir](/kb/en/aria-system-variables/#aria_sync_log_dir)
- [aria-used-for-temp-tables](/kb/en/aria-system-variables/#aria_used_for_temp_tables)
- [deadlock-search-depth-long](/kb/en/aria-system-variables/#deadlock_search_depth_long)
- [deadlock-search-depth-short](/kb/en/aria-system-variables/#deadlock_search_depth_short)
- [deadlock-timeout-long](/kb/en/aria-system-variables/#deadlock_timeout_long)
- [deadlock-timeout-short](/kb/en/aria-system-variables/#deadlock_timeout_short)

### MyRocks Storage Engine Options

The options and system variables related to the [MyRocks](/columns-storage-engines-and-plugins/storage-engines/myrocks/) storage engine can be found [here](/columns-storage-engines-and-plugins/storage-engines/myrocks/myrocks-system-variables/).

### S3 Storage Engine Options

The options and system variables related to the [S3](/columns-storage-engines-and-plugins/storage-engines/s3-storage-engine/) storage engine can be found [here](/columns-storage-engines-and-plugins/storage-engines/s3-storage-engine/s3-storage-engine-system-variables/).

### CONNECT Storage Engine Options

The options related to the [CONNECT](/columns-storage-engines-and-plugins/storage-engines/connect/) storage engine are described below.

#### CONNECT Storage Engine Options and System Variables

Some options and system variables related to the [CONNECT](/columns-storage-engines-and-plugins/storage-engines/connect/) storage engine can be found [here](/columns-storage-engines-and-plugins/storage-engines/connect/connect-system-variables/). Direct links to many of them can be found below.

- [connect-class-path](/kb/en/connect-system-variables/#connect_class_path)
- [connect-cond-push](/kb/en/connect-system-variables/#connect_cond_push)
- [connect-conv-size](/kb/en/connect-system-variables/#connect_conv_size)
- [connect-default-depth](/kb/en/connect-system-variables/#connect_default_depth)
- [connect-default-prec](/kb/en/connect-system-variables/#connect_default_prec)
- [connect-enable-mongo](/kb/en/connect-system-variables/#connect_enable_mongo)
- [connect-exact-info](/kb/en/connect-system-variables/#connect_exact_info)
- [connect-force_bson](/kb/en/connect-system-variables/#connect_force_bson)
- [connect-indx-map](/kb/en/connect-system-variables/#connect_indx_map)
- [connect-java-wrapper](/kb/en/connect-system-variables/#connect_java_wrapper)
- [connect-json-all-path](/kb/en/connect-system-variables/#connect_json_all_path)
- [connect-json-grp-size](/kb/en/connect-system-variables/#connect_json_grp_size)
- [connect-json-null](/kb/en/connect-system-variables/#connect_json_null)
- [connect-jvm-path](/kb/en/connect-system-variables/#connect_jvm_path)
- [connect-type-conv](/kb/en/connect-system-variables/#connect_type_conv)
- [connect-use-tempfile](/kb/en/connect-system-variables/#connect_use_tempfile)
- [connect-work-size](/kb/en/connect-system-variables/#connect_work_size)
- [connect-xtrace](/kb/en/connect-system-variables/#connect_xtrace)

### Spider Storage Engine Options

The options and system variables related to the [Spider](/columns-storage-engines-and-plugins/storage-engines/spider/) storage engine can be found [here](/columns-storage-engines-and-plugins/storage-engines/spider/spider-server-system-variables/).

### Mroonga Storage Engine Options

The options and system variables related to the [Mroonga](/columns-storage-engines-and-plugins/storage-engines/mroonga/) storage engine can be found [here](/columns-storage-engines-and-plugins/storage-engines/mroonga/mroonga-system-variables/).

### TokuDB Storage Engine Options

The options and system variables related to the [TokuDB](/columns-storage-engines-and-plugins/storage-engines/tokudb/) storage engine can be found [here](/columns-storage-engines-and-plugins/storage-engines/tokudb/tokudb-system-variables/).

## Performance Schema Options

The options related to the [Performance Schema](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/) are described below.



#### `--performance-schema-consumer-events-stages-current`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-consumer-events-stages-current</code>
- <strong>Description:</strong> Enable the [events-stages-current](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-events_stages_current-table/) consumer.
- <strong>Default:</strong> `OFF`

---

#### `--performance-schema-consumer-events-stages-history`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-consumer-events-stages-history</code>
- <strong>Description:</strong> Enable the [events-stages-history](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-events_stages_history-table/) consumer.
- <strong>Default:</strong> `OFF`

---

#### `--performance-schema-consumer-events-stages-history-long`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-consumer-events-stages-history-long</code>
- <strong>Description:</strong> Enable the [events-stages-history-long](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-events_stages_history_long-table/) consumer.
- <strong>Default:</strong> `OFF`

---

#### `--performance-schema-consumer-events-statements-current`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-consumer-events-statements-current</code>
- <strong>Description:</strong> Enable the [events-statements-current](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-events_statements_current-table/) consumer. Use `--skip-performance-schema-consumer-events-statements-current` to disable.
- <strong>Default:</strong> `ON`

---

#### `--performance-schema-consumer-events-statements-history`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-consumer-events-statements-history</code>
- <strong>Description:</strong> Enable the [events-statements-history](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-events_statements_history-table/) consumer.
- <strong>Default:</strong> `OFF`

---

#### `--performance-schema-consumer-events-statements-history-long`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-consumer-events-statements-history-long</code>
- <strong>Description:</strong> Enable the [events-statements-history-long](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-events_statements_history_long-table/) consumer.
- <strong>Default:</strong> `OFF`

---

#### `--performance-schema-consumer-events-waits-current`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-consumer-events-waits-current</code>
- <strong>Description:</strong> Enable the [events-waits-current](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-events_waits_current-table/) consumer.
- <strong>Default:</strong> `OFF`

---

#### `--performance-schema-consumer-events-waits-history`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-consumer-events-waits-history</code>
- <strong>Description:</strong> Enable the [events-waits-history](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-events_waits_history-table/) consumer.
- <strong>Default:</strong> `OFF`

---

#### `--performance-schema-consumer-events-waits-history-long`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-consumer-events-waits-history-long</code>
- <strong>Description:</strong> Enable the [events-waits-history-long](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-events_waits_history_long-table/) consumer.
- <strong>Default:</strong> `OFF`

---

#### `--performance-schema-consumer-global-instrumentation`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-consumer-global-instrumentation</code>
- <strong>Description:</strong> Enable the global-instrumentation consumer. Use `--skip-performance-schema-consumer-global-instrumentation` to disable.
- <strong>Default:</strong> `ON`

---

#### `--performance-schema-consumer-statements-digest`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-consumer-statements-digest</code>
- <strong>Description:</strong> Enable the statements-digest consumer. Use `--skip-performance-schema-consumer-statements-digest` to disable.
- <strong>Default:</strong> `ON`

---

#### `--performance-schema-consumer-thread-instrumentation`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--performance-schema-consumer-thread-instrumentation</code>
- <strong>Description:</strong> Enable the statements-thread-instrumentation. Use `--skip-performance-schema-thread-instrumentation` to disable.
- <strong>Default:</strong> `ON`

---



















### Performance Schema Options and System Variables

Some options and system variables related to the [Performance Schema](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/) can be found [here](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-system-variables/). Direct links to many of them can be found below.

- [performance-schema](/kb/en/performance-schema-system-variables/#performance_schema)
- [performance-schema-accounts-size](/kb/en/performance-schema-system-variables/#performance_schema_accounts_size)
- [performance-schema-digests-size](/kb/en/performance-schema-system-variables/#performance_schema_digests_size)
- [performance-schema-events-stages-history-long-size](/kb/en/performance-schema-system-variables/#performance_schema_events_stages_history_long_size)
- [performance-schema-events-stages-history-size](/kb/en/performance-schema-system-variables/#performance_schema_events_stages_history_size)
- [performance-schema-events-statements-history-long-size](/kb/en/performance-schema-system-variables/#performance_schema_events_statements_history_long_size)
- [performance-schema-events-statements-history-size](/kb/en/performance-schema-system-variables/#performance_schema_events_statements_history_size)
- [performance-schema-events-waits-history-long-size](/kb/en/performance-schema-system-variables/#performance_schema_events_waits_history_long_size)
- [performance-schema-events-waits-history-size](/kb/en/performance-schema-system-variables/#performance_schema_events_waits_history_size)
- [performance-schema-hosts-size](/kb/en/performance-schema-system-variables/#performance_schema_hosts_size)
- [performance-schema-max-cond-classes](/kb/en/performance-schema-system-variables/#performance_schema_max_cond_classes)
- [performance-schema-max-cond-instances](/kb/en/performance-schema-system-variables/#performance_schema_max_cond_instances)
- [performance-schema-max-digest-length](/kb/en/performance-schema-system-variables/#performance_schema_max_digest_length)
- [performance-schema-max-file-classes](/kb/en/performance-schema-system-variables/#performance_schema_max_file_classes)
- [performance-schema-max-file-handles](/kb/en/performance-schema-system-variables/#performance_schema_max_file_handles)
- [performance-schema-max-file-instances](/kb/en/performance-schema-system-variables/#performance_schema_max_file_instances)
- [performance-schema-max-mutex-classes](/kb/en/performance-schema-system-variables/#performance_schema_max_mutex_classes)
- [performance-schema-max-mutex-instances](/kb/en/performance-schema-system-variables/#performance_schema_max_mutex_instances)
- [performance-schema-max-rwlock-classes](/kb/en/performance-schema-system-variables/#performance_schema_max_rwlock_classes)
- [performance-schema-max-rwlock-instances](/kb/en/performance-schema-system-variables/#performance_schema_max_rwlock_instances)
- [performance-schema-max-socket-classes](/kb/en/performance-schema-system-variables/#performance_schema_max_socket_classes)
- [performance-schema-max-socket-instances](/kb/en/performance-schema-system-variables/#performance_schema_max_socket_instances)
- [performance-schema-max-stage-classes](/kb/en/performance-schema-system-variables/#performance_schema_max_stage_classes)
- [performance-schema-max-statement-classes](/kb/en/performance-schema-system-variables/#performance_schema_max_statement_classes)
- [performance-schema-max-table-handles](/kb/en/performance-schema-system-variables/#performance_schema_max_table_handles)
- [performance-schema-max-table-instances](/kb/en/performance-schema-system-variables/#performance_schema_max_table_instances)
- [performance-schema-max-thread-classes](/kb/en/performance-schema-system-variables/#performance_schema_max_thread_classes)
- [performance-schema-max-thread-instances](/kb/en/performance-schema-system-variables/#performance_schema_max_thread_instances)
- [performance-schema-session-connect-attrs-size](/kb/en/performance-schema-system-variables/#performance_schema_session_connect_attrs_size)
- [performance-schema-setup-actors-size](/kb/en/performance-schema-system-variables/#performance_schema_setup_actors_size)
- [performance-schema-setup-objects-size](/kb/en/performance-schema-system-variables/#performance_schema_setup_objects_size)
- [performance-schema-users-size](/kb/en/performance-schema-system-variables/#performance_schema_users_size)

## Galera Cluster Options

The options related to [Galera Cluster](/kb/en/galera/) are described below.

#### `--wsrep-new-cluster`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--wsrep-new-cluster</code>
- <strong>Description:</strong> Bootstrap a cluster. It works by overriding the current value of wsrep_cluster_address. It is recommended not to add this option to the config file as this will trigger bootstrap on every server start.

---

### Galera Cluster Options and System Variables

Some options and system variables related to [Galera Cluster](/kb/en/galera/) can be found [here](/replication/galera-cluster/galera-cluster-system-variables/). Direct links to many of them can be found below.

- [wsrep-auto-increment-control](/kb/en/galera-cluster-system-variables/#wsrep_auto_increment_control)
- [wsrep-causal-reads](/kb/en/galera-cluster-system-variables/#wsrep_causal_reads)
- [wsrep-certify-nonPK](/kb/en/galera-cluster-system-variables/#wsrep_certify_nonpk)
- [wsrep-cluster-address](/kb/en/galera-cluster-system-variables/#wsrep_cluster_address)
- [wsrep-cluster-name](/kb/en/galera-cluster-system-variables/#wsrep_cluster_name)
- [wsrep-convert-LOCK-to-trx](/kb/en/galera-cluster-system-variables/#wsrep_convert_lock_to_trx)
- [wsrep-data-home-dir](/kb/en/galera-cluster-system-variables/#wsrep_data_home_dir)
- [wsrep-dbug-option](/kb/en/galera-cluster-system-variables/#wsrep_dbug_option)
- [wsrep-debug](/kb/en/galera-cluster-system-variables/#wsrep_debug)
- [wsrep-desync](/kb/en/galera-cluster-system-variables/#wsrep_desync)
- [wsrep-dirty-reads](/kb/en/galera-cluster-system-variables/#wsrep_dirty_reads)
- [wsrep-drupal-282555-workaround](/kb/en/galera-cluster-system-variables/#wsrep_drupal_282555_workaround)
- [wsrep-forced-binlog-format](/kb/en/galera-cluster-system-variables/#wsrep_forced_binlog_format)
- [wsrep-gtid-domain-id](/kb/en/galera-cluster-system-variables/#wsrep_gtid_domain_id)
- [wsrep-gtid-mode](/kb/en/galera-cluster-system-variables/#wsrep_gtid_mode)
- [wsrep-ignore-apply-errors](/kb/en/galera-cluster-system-variables/#wsrep_ignore_apply_errors)
- [wsrep-load-data-splitting](/kb/en/galera-cluster-system-variables/#wsrep_load_data_splitting)
- [wsrep-log-conflicts](/kb/en/galera-cluster-system-variables/#wsrep_log_conflicts)
- [wsrep-max-ws-rows](/kb/en/galera-cluster-system-variables/#wsrep_max_ws_rows)
- [wsrep-max-ws-size](/kb/en/galera-cluster-system-variables/#wsrep_max_ws_size)
- [wsrep-mode](/kb/en/galera-cluster-system-variables/#wsrep_mode)
- [wsrep-mysql-replication-bundle](/kb/en/galera-cluster-system-variables/#wsrep_mysql_replication_bundle)
- [wsrep-node-address](/kb/en/galera-cluster-system-variables/#wsrep_node_address)
- [wsrep-node-incoming-address](/kb/en/galera-cluster-system-variables/#wsrep_node_incoming_address)
- [wsrep-node-name](/kb/en/galera-cluster-system-variables/#wsrep_node_name)
- [wsrep-notify-cmd](/kb/en/galera-cluster-system-variables/#wsrep_notify_cmd)
- [wsrep-on](/kb/en/galera-cluster-system-variables/#wsrep_on)
- [wsrep-OSU-method](/kb/en/galera-cluster-system-variables/#wsrep_osu_method)
- [wsrep-provider](/kb/en/galera-cluster-system-variables/#wsrep_provider)
- [wsrep-provider-options](/kb/en/galera-cluster-system-variables/#wsrep_provider_options)
- [wsrep-recover](/kb/en/galera-cluster-system-variables/#wsrep_recover)
- [wsrep-reject_queries](/kb/en/galera-cluster-system-variables/#wsrep_reject_queries)
- [wsrep-retry-autocommit](/kb/en/galera-cluster-system-variables/#wsrep_retry_autocommit)
- [wsrep-slave-FK-checks](/kb/en/galera-cluster-system-variables/#wsrep_slave_fk_checks)
- [wsrep-slave-threads](/kb/en/galera-cluster-system-variables/#wsrep_slave_threads)
- [wsrep-slave-UK-checks](/kb/en/galera-cluster-system-variables/#wsrep_slave_uk_checks)
- [wsrep-sr-store](/kb/en/galera-cluster-system-variables/#wsrep_sr_store)
- [wsrep-sst-auth](/kb/en/galera-cluster-system-variables/#wsrep_sst_auth)
- [wsrep-sst-donor](/kb/en/galera-cluster-system-variables/#wsrep_sst_donor)
- [wsrep-sst-donor-rejects-queries](/kb/en/galera-cluster-system-variables/#wsrep_sst_donor_rejects_queries)
- [wsrep-sst-method](/kb/en/galera-cluster-system-variables/#wsrep_sst_method)
- [wsrep-sst-receive-address](/kb/en/galera-cluster-system-variables/#wsrep_sst_receive_address)
- [wsrep-start-position](/kb/en/galera-cluster-system-variables/#wsrep_start_position)
- [wsrep-strict-ddl](/kb/en/galera-cluster-system-variables/#wsrep_strict_ddl)
- [wsrep-sync-wait](/kb/en/galera-cluster-system-variables/#wsrep_sync_wait)
- [wsrep-trx_fragment_size](/kb/en/galera-cluster-system-variables/#wsrep_trx_fragment_size)
- [wsrep-trx_fragment_unit](/kb/en/galera-cluster-system-variables/#wsrep_trx_fragment_unit)

## Options When Debugging mysqld

#### `--debug-assert-if-crashed-table`

- <strong>Description:</strong> Do an assert in handler::print_error() if we get a crashed table.

---

#### `--debug-binlog-fsync-sleep`

- <strong>Description:</strong> <code class="fixed" style="white-space:pre-wrap">--debug-binlog-fsync-sleep=#</code>If not set to zero, sets the number of micro-seconds to sleep after running fsync() on the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) to flush transactions to disk. This can thus be used to artificially increase the perceived cost of such an fsync().

---

#### `--debug-crc-break`

- <strong>Description:</strong> <code class="fixed" style="white-space:pre-wrap">--debug-crc-break=#</code>Call my_debug_put_break_here() if crc matches this number (for debug).

---

#### `--debug-flush`

- <strong>Description:</strong> Default debug log with flush after write.

---

#### `--debug-no-sync`

- <strong>Description:</strong> <code class="fixed" style="white-space:pre-wrap">debug-no-sync[=#]</code>Disables system sync calls. Only for running tests or debugging!

---

#### `--debug-sync-timeout`

- <strong>Description:</strong> <code class="fixed" style="white-space:pre-wrap">debug-sync-timeout[=#]</code>Enable the debug sync facility and optionally specify a default wait timeout in seconds. A zero value keeps the facility disabled.

---

#### `--gdb`

- <strong>Description:</strong> Set up signals usable for debugging.

---

#### `--silent-startup`

- <strong>Description:</strong> Don't print Notes to the [error log](/mariadb-administration/server-monitoring-logs/error-log/) during startup.
- <strong>Introduced:</strong> [MariaDB 10.1.8](/kb/en/mariadb-1018-release-notes/)

---

#### `--sync-sys`

- <strong>Description:</strong> Enable/disable system sync calls. Syncs should only be turned off (<code class="fixed" style="white-space:pre-wrap">--disable-sync-sys</code>) when running tests or debugging! Replaced by [debug-no-sync](#-debug-no-sync) from [MariaDB 5.5](/kb/en/what-is-mariadb-55/).
- <strong>Removed:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/)

---

#### `--thread-alarm`

- <strong>Description:</strong> Enable/disable system thread alarm calls. Should only be turned off (<code class="fixed" style="white-space:pre-wrap">--disable-thread-alarm</code>) when running tests or debugging!

---

### Debugging Options and System Variables

- [core-file](/kb/en/server-system-variables/#core_file)
- [debug](/kb/en/server-system-variables/#debug)
- [debug-no-thread-alarm](/kb/en/server-system-variables/#debug_no_thread_alarm)

## Other Options

#### `--allow-suspicious-udfs`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--allow-suspicious-udfs</code>
- <strong>Description:</strong> Allows use of [user-defined functions](/programming-customizing-mariadb/user-defined-functions/) consisting of only one symbol `x()` without corresponding `x_init()` or `x_deinit()`. That also means that one can load any function from any library, for example `exit()` from `libc.so`. Not recommended unless you require old UDF's with one symbol that cannot be recompiled

---

#### `--bootstrap`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--bootstrap</code>
- <strong>Description:</strong> Used by mysql installation scripts, such as [mysql_install_db](/clients-utilities/mysql_install_db/) to execute SQL scripts before any privilege or system tables exist. Do no use while an existing MariaDB instance is running.

---

#### `--chroot`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--chroot=name</code>
- <strong>Description:</strong> Chroot mysqld daemon during startup.

---

#### `--des-key-file`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--des-key-file=name</code>
- <strong>Description:</strong> Load keys for [des_encrypt()](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/des_encrypt/) and des_encrypt from given file.

---

#### `--exit-info`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--exit-info[=#]</code>
- <strong>Description:</strong> Used for debugging. Use at your own risk.

---

#### `--getopt-prefix-matching`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--getopt-prefix-matching={0|1}</code>
- <strong>Description:</strong> Makes it possible to disable historical "unambiguous prefix" matching in the command-line option parsing.
- <strong>Default:</strong> TRUE
- <strong>Introduced:</strong> [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)

---

#### `--help`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--help</code>
- <strong>Description:</strong> Displays help with many commandline options described, and exits.

---

#### `--log-short-format`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--log-short-format</code>
- <strong>Description:</strong>  Don't log extra information to update and [slow-query](/mariadb-administration/server-monitoring-logs/slow-query-log/) logs.

---

#### `--log-slow-file`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--log-slow-file=name</code>
- <strong>Description:</strong> Log [slow queries](/mariadb-administration/server-monitoring-logs/slow-query-log/) to given log file. Defaults logging to hostname-slow.log

---

#### `--log-slow-time`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--log-slow-time=#</code>
- <strong>Description:</strong> Log all queries that have taken more than [long-query-time](/kb/en/server-system-variables/#long_query_time) seconds to execute to the slow query log, if active. The argument will be treated as a decimal value with microsecond precision.

---

#### `--log-tc`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--log-tc=name</code>
- <strong>Description:</strong> Defines the path to the memory-mapped file-based transaction coordinator log, which is only used if the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) is disabled. If you have two or more XA-capable storage engines enabled, then a transaction coordinator log must be available. See [Transaction Coordinator Log](/mariadb-administration/server-monitoring-logs/transaction-coordinator-log/) for more information. Also see the the <a undefined>log_tc_size</a> system variable and the <a undefined>--tc-heuristic-recover</a> option.
- <strong>Default Value:</strong> `tc.log`

---

#### `--master-connect-retry`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--master-connect-retry=#</code>
- <strong>Description:</strong> Deprecated in 5.1.17 and removed in 5.5. The number of seconds the slave thread will sleep before retrying to connect to the master, in case the master goes down or the connection is lost.

---

#### `--memlock`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--memlock</code>
- <strong>Description:</strong> Lock mysqld in memory.

---

#### `--ndb-use-copying-alter-table`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--ndb-use-copying-alter-table</code>
- <strong>Description:</strong> Force ndbcluster to always copy tables at alter table (should only be used if on-line alter table fails).

---

#### `--one-thread`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--one-thread</code>
- <strong>Description:</strong> (Deprecated): Only use one thread (for debugging under Linux). Use [thread-handling=no-threads](/kb/en/thread-pool-system-and-status-variables/#thread_handling) instead.
- <strong>Removed:</strong> [MariaDB 10.0.4](/kb/en/mariadb-1004-release-notes/)

---

#### `--plugin-load`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--plugin-load=name</code>
- <strong>Description:</strong> This option can be used to configure the server to load specific [plugins](/kb/en/mariadb-plugins/). This option uses the following format:
<ul start="1"><li>Plugins can be specified in the format `name=library`, where `name` is the plugin name and `library` is the plugin library. This format installs a single plugin from the given plugin library.
</li><li>Plugins can also be specified in the format `library`, where `library` is the plugin library. This format installs all plugins from the given plugin library.
</li><li>Multiple plugins can be specified by separating them with semicolons.
</li></ul>
- Special care must be taken when specifying the <a undefined>--plugin-load</a> option multiple times, or when specifying both the <a undefined>--plugin-load</a> option and the <a undefined>--plugin-load-add</a> option together. The <a undefined>--plugin-load</a> option resets the plugin load list, and this can cause unexpected problems if you are not aware. The <a undefined>--plugin-load-add</a> option does <strong>not</strong> reset the plugin load list, so it is much safer to use. See [Plugin Overview: Specifying Multiple Plugin Load Options](/kb/en/plugin-overview/#specifying-multiple-plugin-load-options) for more information.
- See [Plugin Overview: Installing a Plugin with Plugin Load Options](/kb/en/plugin-overview/#installing-a-plugin-with-plugin-load-options) for more information.

---

#### `--plugin-load-add`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--plugin-load-add=name</code>
- <strong>Description:</strong> This option can be used to configure the server to load specific [plugins](/kb/en/mariadb-plugins/). This option uses the following format:
<ul start="1"><li>Plugins can be specified in the format `name=library`, where `name` is the plugin name and `library` is the plugin library. This format installs a single plugin from the given plugin library.
</li><li>Plugins can also be specified in the format `library`, where `library` is the plugin library. This format installs all plugins from the given plugin library.
</li><li>Multiple plugins can be specified by separating them with semicolons.
</li></ul>
- Special care must be taken when specifying both the <a undefined>--plugin-load</a> option and the <a undefined>--plugin-load-add</a> option together. The <a undefined>--plugin-load</a> option resets the plugin load list, and this can cause unexpected problems if you are not aware. The <a undefined>--plugin-load-add</a> option does <strong>not</strong> reset the plugin load list, so it is much safer to use. See [Plugin Overview: Specifying Multiple Plugin Load Options](/kb/en/plugin-overview/#specifying-multiple-plugin-load-options) for more information.
- See [Plugin Overview: Installing a Plugin with Plugin Load Options](/kb/en/plugin-overview/#installing-a-plugin-with-plugin-load-options) for more information.
- <strong>Introduced:</strong> [MariaDB 10.0.1](/kb/en/mariadb-1001-release-notes/)

---

#### `--port-open-timeout`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--port-open-timeout=#</code>
- <strong>Description:</strong> Maximum time in seconds to wait for the port to become free. (Default: No wait).

---

&lt;&lt;toc-item slug="server-system-variables#proxy_protocol_networks
" level=3&gt;&gt;`--proxy-protocol-networks`&lt;&lt;/toc-item&gt;&gt;

#### `--safe-user-create`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--safe-user-create</code>
- <strong>Description:</strong> Don't allow new user creation by the user who has no write privileges to the [mysql.user](/kb/en/mysqluser-table/) table.

---

#### `--safemalloc-mem-limit`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--safemalloc-mem-limit=#</code>
- <strong>Description:</strong> Simulate memory shortage when compiled with the `<code>--`with-debug=full</code> option.

---

#### `--show-slave-auth-info`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--show-slave-auth-info</code>
- <strong>Description:</strong> Show user and password in SHOW SLAVE HOSTS on this master.

---

#### `--skip-grant-tables`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--skip-grant-tables</code>
- <strong>Description:</strong> Start without grant tables. This gives all users FULL ACCESS to all tables, which is useful in case of a lost root password. Use [mysqladmin flush-privileges](/clients-utilities/mysqladmin/), [mysqladmin reload](/clients-utilities/mysqladmin/)  or [FLUSH PRIVILEGES](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/) to resume using the grant tables.

---

#### `--skip-host-cache`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--skip-host-cache</code>
- <strong>Description:</strong> Don't cache host names.

---

#### `--skip-partition`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--skip-partition</code>, <code class="fixed" style="white-space:pre-wrap">--disable-partition</code>
- <strong>Description:</strong> Disables user-defined [partitioning](/kb/en/managing-mariadb-partitioning/). Previously partitioned tables cannot be accessed or modifed. Tables can still be seen with [SHOW TABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-tables/) or by viewing the [INFORMATION_SCHEMA.TABLES table](/kb/en/information-schema-tables-table/). Tables can be dropped with [DROP TABLE](/sql-statements-structure/sql-statements/data-definition/drop/drop-table/), but this only removes .frm files, not the associated .par files, which will need to be removed manually.

---

#### `--skip-slave-start`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--skip-slave-start</code>
- <strong>Description:</strong> If set, slave is not autostarted.

---

#### `--skip-ssl`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--skip-ssl</code>
- <strong>Description:</strong> Disable [TLS connections](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/secure-connections-overview/).

---

#### `--skip-symlink`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--skip-symlink</code>
- <strong>Description:</strong> Don't allow symlinking of tables. Deprecated and removed in [MariaDB 5.5](/kb/en/what-is-mariadb-55/). Use [symbolic-links](#-symbolic-links) with the `skip` [option prefix](#option-prefixes) instead.
- <strong>Removed:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/)

---

#### `--skip-thread-priority`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--skip-thread-priority</code>
- <strong>Description:</strong> Don't give threads different priorities. Deprecated and removed in [MariaDB 10.0](/kb/en/what-is-mariadb-100/).
- <strong>Removed:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `--sql-bin-update-same`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--sql-bin-update-same=#</code>
- <strong>Description:</strong> The update log was deprecated in version 5.0 and replaced by the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/), so this option did nothing since then. Deprecated and removed in [MariaDB 5.5](/kb/en/what-is-mariadb-55/).
- <strong>Removed:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/)

---

#### `--ssl`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--ssl</code>
- <strong>Description:</strong> Enable [TLS for connection](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/secure-connections-overview/) (automatically enabled with other flags). Disable with '`<code>--`skip-ssl</code>'.

---

#### `--stack-trace`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--stack-trace</code>, <code class="fixed" style="white-space:pre-wrap">--skip-stack-trace</code>
- <strong>Description:</strong> Print a stack trace on failure. Enabled by default, disable with `-skip-stack-trace`.

---

#### `--symbolic-links`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--symbolic-links</code>
- <strong>Description:</strong> Enables symbolic link support. When set, the <a undefined>have_symlink</a> system variable shows as `YES`. Silently ignored in Windows.

---

#### `--tc-heuristic-recover`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--tc-heuristic-recover=name</code>
- <strong>Description:</strong> If [manual heuristic recovery](/mariadb-administration/server-monitoring-logs/transaction-coordinator-log/heuristic-recovery-with-the-transaction-coordinator-log/) is needed, this option defines the decision to use in the heuristic recovery process. Manual heuristic recovery may be needed if the [transaction coordination log](/mariadb-administration/server-monitoring-logs/transaction-coordinator-log/) is missing or if it doesn't contain all prepared transactions. This option can be set to `OFF`, `COMMIT`, or `ROLLBACK`. The default is `OFF`. See also the <a undefined>--log-tc</a> server option and the <a undefined>log_tc_size</a> system variable.

---

#### `--temp-pool`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--temp-pool</code>
- <strong>Description:</strong> Using this option will cause most temporary files created to use a small set of names, rather than a unique name for each new file. Defaults to `1` until [MariaDB 10.5.6](/kb/en/mariadb-1056-release-notes/), use `--skip-temp-pool` to disable. Deprecated and defaults to `0` from [MariaDB 10.5.7](/kb/en/mariadb-1057-release-notes/), as benchmarking shows it causes a heavy mutex contention.

---

#### `--test-expect-abort`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--test-expect-abort</code>
- <strong>Description:</strong> Expect that server aborts with 'abort'; Don't write out server variables on 'abort'. Useful only for test scripts.

---

#### `--test-ignore-wrong-options`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--test-ignore-wrong-options</code>
- <strong>Description:</strong> Ignore wrong enums values in command line arguments. Useful only for test scripts.

---

#### `--user`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--user=name</code>
- <strong>Description:</strong> Run mysqld daemon as user.

---

#### `--verbose`

- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">-v</code>,  <code class="fixed" style="white-space:pre-wrap">--verbose</code>
- <strong>Description:</strong> Used with [help](#help) option for detailed help.

---

## Other Options and System Variables

- [allow-suspicious-udfs](/kb/en/server-system-variables/#allow-suspicious-udfs)
- [automatic-sp-privileges](/kb/en/server-system-variables/#automatic_sp_privileges)
- [back-log](/kb/en/server-system-variables/#back_log)
- [basedir](/kb/en/server-system-variables/#basedir)
- [check-constraint-checks](/kb/en/server-system-variables/#check_constraint_checks)
- [column-compression-threshold](/kb/en/storage-engine-independent-column-compression/#column_compression_threshold)
- [column-compression-zlib-level](/kb/en/storage-engine-independent-column-compression/#column_compression_zlib_level)
- [column-compression-zlib-strategy](/kb/en/storage-engine-independent-column-compression/#column_compression_zlib_strategy)
- [column-compression-zlib-wrap](/kb/en/storage-engine-independent-column-compression/#column_compression_zlib_wrap)
- [completion-type](/kb/en/server-system-variables/#completion_type)
- [connect-timeout](/kb/en/server-system-variables/#connect_timeout)
- [datadir](/kb/en/server-system-variables/#datadir)
- [date-format](/kb/en/server-system-variables/#date_format)
- [datetime-format](/kb/en/server-system-variables/#datetime_format)
- [deadlock-search-depth-long](/kb/en/aria-server-system-variables/#deadlock_search_depth_long)
- [deadlock-search-depth-short](/kb/en/aria-server-system-variables/#deadlock_search_depth_short)
- [deadlock-timeout-long](/kb/en/aria-server-system-variables/#deadlock_timeout_long)
- [deadlock-timeout-short](/kb/en/aria-server-system-variables/#deadlock_timeout_short)
- [default-password-lifetime](/kb/en/server-system-variables/#default_password_lifetime)
- [default-regex-flags](/kb/en/server-system-variables/#default_regex_flags)
- [default-storage-engine](/kb/en/server-system-variables/#default_storage_engine)
- [default-table-type](/kb/en/server-system-variables/#default_table_type)
- [delay-key-write](/kb/en/server-system-variables/#delay_key_write)
- [disconnect-on-expired-password](/kb/en/server-system-variables/#disconnect_on_expired_password)
- [div-precision-increment](/kb/en/server-system-variables/#div_precision_increment)
- [enable-named-pipe](/kb/en/server-system-variables/#named_pipe)
- [encrypt-binlog](/kb/en/server-system-variables/#encrypt_binlog)
- [encrypt-tmp-disk-tables](/kb/en/server-system-variables/#encrypt_tmp_disk_tables)
- [encrypt-tmp-files](/kb/en/server-system-variables/#encrypt_tmp_files)
- [encryption-algorithm](/kb/en/server-system-variables/#encryption_algorithm)
- [engine-condition-pushdown](/kb/en/server-system-variables/#engine_condition_pushdown)
- [eq-range-index-dive-limit](/kb/en/server-system-variables/#eq_range_index_dive_limit)
- [event-scheduler](/kb/en/server-system-variables/#event_scheduler)
- [expire-logs-days](/kb/en/server-system-variables/#expire_logs_days)
- [explicit-defaults-for-timestamp](/kb/en/server-system-variables/#explicit_defaults_for_timestamp)
- [extra-max-connections](/kb/en/thread-pool-system-and-status-variables/#extra_max_connections)
- [extra-port](/kb/en/thread-pool-system-and-status-variables/#extra_port)
- [flush](/kb/en/server-system-variables/#flush)
- [flush-time](/kb/en/server-system-variables/#flush_time)
- [ft-boolean-syntax](/kb/en/server-system-variables/#ft_boolean_syntax)
- [ft-max-word-len](/kb/en/server-system-variables/#ft_max_word_len)
- [ft-min-word-len](/kb/en/server-system-variables/#ft_min_word_len)
- [ft-query-expansion-limit](/kb/en/server-system-variables/#ft_query_expansion_limit)
- [ft-stopword-file](/kb/en/server-system-variables/#ft_stopword_file)
- [general-log](/kb/en/server-system-variables/#general_log)
- [general-log-file](/kb/en/server-system-variables/#general_log_file)
- [group-concat-max-len](/kb/en/server-system-variables/#group_concat_max_len)
- [histogram-size](/kb/en/server-system-variables/#histogram_size)
- [histogram-type](/kb/en/server-system-variables/#histogram_type)
- [host-cache-size](/kb/en/server-system-variables/#host_cache_size)
- [idle-readonly-transaction-timeout](/kb/en/server-system-variables/#idle_readonly_transaction_timeout)
- [idle-transaction-timeout](/kb/en/server-system-variables/#idle_transaction_timeout)
- [idle-write-transaction-timeout](/kb/en/server-system-variables/#idle_write_transaction_timeout)
- [ignore-db-dirs](/kb/en/server-system-variables/#ignore_db_dirs)
- [in-predicate-conversion-threshold](/kb/en/server-system-variables/#in_predicate_conversion_threshold)
- [init-connect](/kb/en/server-system-variables/#init_connect)
- [init-file](/kb/en/server-system-variables/#init_file)
- [interactive-timeout](/kb/en/server-system-variables/#interactive_timeout)
- [large-pages](/kb/en/server-system-variables/#large_pages)
- [local-infile](/kb/en/server-system-variables/#local_infile)
- [lock-wait-timeout](/kb/en/server-system-variables/#lock_wait_timeout)
- [log](/kb/en/server-system-variables/#log)
- [log-disabled-statements](/kb/en/server-system-variables/#log_disabled_statements)
- [log-error](/kb/en/server-system-variables/#log_error)
- [log-output](/kb/en/server-system-variables/#log_output)
- [log-queries-not-using-indexes](/kb/en/server-system-variables/#log_queries_not_using_indexes)
- [log-slow-admin-statements](/kb/en/server-system-variables/#log_slow_admin_statements)
- [log-slow-disabled-statements](/kb/en/server-system-variables/#log_slow_disabled_statements)
- [log-slow-filter](/kb/en/server-system-variables/#log_slow_filter)
- [log-slow-queries](/kb/en/server-system-variables/#log_slow_queries)
- [log-slow-rate-limit](/kb/en/server-system-variables/#log_slow_rate_limit)
- [log-slow-verbosity](/kb/en/server-system-variables/#log_slow_verbosity)
- [log-tc-size](/kb/en/server-system-variables/#log_tc_size)
- [log-warnings](/kb/en/server-system-variables/#log_warnings)
- [long-query-time](/kb/en/server-system-variables/#long_query_time)
- [low-priority-updates](/kb/en/server-system-variables/#low_priority_updates)
- [lower-case-table-names](/kb/en/server-system-variables/#lower_case_table_names)
- [max-allowed-packet](/kb/en/server-system-variables/#max_allowed_packet)
- [max-connections](/kb/en/server-system-variables/#max_connections)
- [max-connect-errors](/kb/en/server-system-variables/#max_connect_errors)
- [max-delayed-threads](/kb/en/server-system-variables/#max_delayed_threads)
- [max-digest-length](/kb/en/server-system-variables/#max_digest_length%22)
- [max-error-count](/kb/en/server-system-variables/#max_error_count)
- [max-length-for-sort-data](/kb/en/server-system-variables/#max_length_for_sort_data)
- [max-long-data-size](/kb/en/server-system-variables/#max_long_data_size)
- [max-password-errors](/kb/en/server-system-variables/#max_password_errors)
- [max-prepared-stmt-count](/kb/en/server-system-variables/#max_prepared_stmt_count)
- [max-recursive-iterations](/kb/en/server-system-variables/#max_recursive_iterations)
- [max-rowid-filter-size](/kb/en/server-system-variables/#max_rowid_filter_size)
- [max-session-mem-used](/kb/en/server-system-variables/#max_session_mem_used)
- [max-sp-recursion-depth](/kb/en/server-system-variables/#max_sp_recursion_depth)
- [max-statement-time](/kb/en/server-system-variables/#max_statement_time)
- [max-tmp-tables](/kb/en/server-system-variables/#max_tmp_tables)
- [max-user-connections](/kb/en/server-system-variables/#max_user_connections)
- [max-write-lock-count](/kb/en/server-system-variables/#max_write_lock_count)
- [metadata-locks-cache-size](/kb/en/server-system-variables/#metadata_locks_cache_size)
- [metadata-locks-hash-instances](/kb/en/server-system-variables/#metadata_locks_hash_instances)
- [min-examined-row-limit](/kb/en/server-system-variables/#min_examined_row_limit)
- [mrr-buffer-size](/kb/en/server-system-variables/#mrr_buffer_size)
- [multi-range-count](/kb/en/server-system-variables/#multi_range_count)
- [--mysql56-temporal-format](/kb/en/server-system-variables/#mysql56_temporal_format)
- [net-buffer-length](/kb/en/server-system-variables/#net_buffer_length)
- [net-read-timeout](/kb/en/server-system-variables/#net_read_timeout)
- [net-retry-count](/kb/en/server-system-variables/#net_retry_count)
- [net-write-timeout](/kb/en/server-system-variables/#net_write_timeout)
- [open-files-limit](/kb/en/server-system-variables/#open_files_limit)
- [pid-file](/kb/en/server-system-variables/#pid_file)
- [plugin-dir](/kb/en/server-system-variables/#plugin_dir)
- [plugin-maturity](/kb/en/server-system-variables/#plugin_maturity)
- [port](/kb/en/server-system-variables/#port)
- [preload-buffer-size](/kb/en/server-system-variables/#preload_buffer_size)
- [profiling-history-size](/kb/en/server-system-variables/#profiling_history_size)
- [progress-report-time](/kb/en/server-system-variables/#progress_report_time)
- [proxy-protocol-networks](/kb/en/server-system-variables/#proxy_protocol_networks)
- [query-cache-limit](/kb/en/server-system-variables/#query_cache_limit)
- [query-cache-min-res-unit](/kb/en/server-system-variables/#query_cache_min_res_unit)
- [query-cache-strip-comments](/kb/en/server-system-variables/#query_cache_strip_comments)
- [query-cache-wlock-invalidate](/kb/en/server-system-variables/#query_cache_wlock_invalidate)
- [read-rnd-buffer-size](/kb/en/server-system-variables/#read_rnd_buffer_size)
- [read-only](/kb/en/server-system-variables/#read_only)
- [require-secure-transport](/kb/en/server-system-variables/#require_secure_transport)
- [safe-show-database](/kb/en/server-system-variables/#safe_show_database)
- [secure-auth](/kb/en/server-system-variables/#secure_auth)
- [secure-file-priv](/kb/en/server-system-variables/#secure_file_priv)
- [secure-timestamp](/kb/en/server-system-variables/#secure_timestamp)
- [session-track-schema](/kb/en/server-system-variables/#session_track_schema)
- [session-track-state-change](/kb/en/server-system-variables/#session_track_state_change)
- [session-track-system-variables](/kb/en/server-system-variables/#session_track_system_variables)
- [session-track-transaction-info](/kb/en/server-system-variables/#session_track_transaction_info)
- [skip-automatic-sp-privileges](/kb/en/server-system-variables/#automatic_sp_privileges)
- [skip-external-locking](/kb/en/server-system-variables/#skip_external_locking)
- [skip-large-pages](/kb/en/server-system-variables/#large_pages)
- [skip-log-error](/kb/en/server-system-variables/#log_error)
- [skip-name-resolve](/kb/en/server-system-variables/#skip_name_resolve)
- [skip-networking](/kb/en/server-system-variables/#skip_networking)
- [skip-show-database](/kb/en/server-system-variables/#skip_show_database)
- [slow-launch-time](/kb/en/server-system-variables/#slow_launch_time)
- [slow-query-log](/kb/en/server-system-variables/#slow_query_log)
- [slow-query-log-file](/kb/en/server-system-variables/#slow_query_log_file)
- [socket](/kb/en/server-system-variables/#socket)
- [sort-buffer-size](/kb/en/server-system-variables/#sort_buffer_size)
- [sql-if-exists](/kb/en/server-system-variables/#sql_if_exists)
- [sql-mode](/kb/en/server-system-variables/#sql_mode)
- [ssl-ca](/kb/en/ssl-server-system-variables/#ssl_ca)
- [ssl-capath](/kb/en/ssl-server-system-variables/#ssl_capath)
- [ssl-cert](/kb/en/ssl-server-system-variables/#ssl_cert)
- [ssl-cipher](/kb/en/ssl-server-system-variables/#ssl_cipher)
- [ssl-crl](/kb/en/ssl-server-system-variables/#ssl_crl)
- [ssl-crlpath](/kb/en/ssl-server-system-variables/#ssl_crlpath)
- [ssl-key](/kb/en/ssl-server-system-variables/#ssl_key)
- [standards_compliant_cte](/kb/en/server-system-variables/#standards_compliant_cte)
- [stored-program-cache](/kb/en/server-system-variables/#stored_program_cache)
- [strict_password_validation](/kb/en/server-system-variables/#strict_password_validation)
- [sync-frm](/kb/en/server-system-variables/#sync_frm)
- [system-versioning-alter-history](/kb/en/system-versioned-tables/#system_versioning_alter_history)
- [system-versioning-asof](/kb/en/system-versioned-tables/#system_versioning_asof)
- [system-versioning-innodb-algorithm-simple](/kb/en/system-versioned-tables/#system_versioning_innodb_algorithm_simple)
- [table-lock-wait-timeout](/kb/en/server-system-variables/#table_lock_wait_timeout)
- [tcp-keepalive-interval](/kb/en/server-system-variables/#tcp_keepalive_interval)
- [tcp-keepalive-probes](/kb/en/server-system-variables/#tcp_keepalive_probes)
- [tcp-keepalive-time](/kb/en/server-system-variables/#tcp_keepalive_time)
- [tcp-nodelay](/kb/en/server-system-variables/#tcp_nodelay)
- [thread-cache-size](/kb/en/server-system-variables/#thread_cache_size)
- [thread-concurrency](/kb/en/server-system-variables/#thread_concurrency)
- [thread-handling](/kb/en/server-system-variables/#thread_handling)
- [thread-pool-dedicated-listener](/kb/en/thread-pool-system-and-status-variables/#thread_pool_dedicated_listener)
- [thread-pool-exact-stats](/kb/en/thread-pool-system-and-status-variables/#thread_pool_exact_stats)
- [thread-pool-idle-timeout](/kb/en/thread-pool-system-and-status-variables/#thread_pool_idle_timeout)
- [thread-pool-max-threads](/kb/en/thread-pool-system-and-status-variables/#thread_pool_max_threads)
- [thread-pool-min-threads](/kb/en/thread-pool-system-and-status-variables/#thread_pool_min_threads)
- [thread-pool-oversubscribe](/kb/en/thread-pool-system-and-status-variables/#thread_pool_oversubscribe)
- [thread-pool-prio-kickup-timer](/kb/en/thread-pool-system-and-status-variables/#thread_pool_prio_kickup_timer)
- [thread-pool-priority](/kb/en/thread-pool-system-and-status-variables/#thread_pool_priority)
- [thread-pool-size](/kb/en/thread-pool-system-and-status-variables/#thread_pool_size)
- [thread-pool-stall-limit](/kb/en/thread-pool-system-and-status-variables/#thread_pool_stall_limit)
- [thread-stack](/kb/en/server-system-variables/#thread_stack)
- [timed-mutexes](/kb/en/server-system-variables/#timed_mutexes)
- [time-format](/kb/en/server-system-variables/#time_format)
- [tls-version](/kb/en/ssl-server-system-variables/#tls_version)
- [tmpdir](/kb/en/server-system-variables/#tmpdir)
- [transaction-isolation](/kb/en/server-system-variables/#tx_isolation)
- [transaction-alloc-block-size](/kb/en/server-system-variables/#transaction_alloc_block_size)
- [transaction-prealloc-size](/kb/en/server-system-variables/#transaction_prealloc_size)
- [transaction-read-only](/kb/en/server-system-variables/#tx_read_only)
- [updatable-views-with-limit](/kb/en/server-system-variables/#updatable_views_with_limit)
- [userstat](/kb/en/server-system-variables/#userstat)
- [version](/kb/en/server-system-variables/#version)
- [wait-timeout](/kb/en/server-system-variables/#wait_timeout)

## Authentication Plugins - Options and System Variables

### Authentication Plugin - `ed25519`

The options related to the [ed25519](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-ed25519/) authentication plugin can be found [here](/kb/en/authentication-plugin-ed25519/#options).

### Authentication Plugin - `gssapi`

The system variables related to the [gssapi](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-gssapi/) authentication plugin can be found [here](/kb/en/authentication-plugin-gssapi/#system-variables).

The options related to the [gssapi](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-gssapi/) authentication plugin can be found [here](/kb/en/authentication-plugin-gssapi/#options).

### Authentication Plugin - `named_pipe`

The options related to the [named_pipe](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-named-pipe/) authentication plugin can be found [here](/kb/en/authentication-plugin-named-pipe/#options).

### Authentication Plugin - `pam`

The system variables related to the [pam](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-with-pluggable-authentication-modules-pam/authentication-plugin-pam/) authentication plugin can be found [here](/kb/en/authentication-plugin-pam/#system-variables).

The options related to the [pam](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-with-pluggable-authentication-modules-pam/authentication-plugin-pam/) authentication plugin can be found [here](/kb/en/authentication-plugin-pam/#options).

### Authentication Plugin - `unix_socket`

The options related to the [unix_socket](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-unix-socket/) authentication plugin can be found [here](/kb/en/authentication-plugin-unix-socket/#options).

## Encryption Plugins - Options and System Variables

### Encryption Plugin - `aws_key_management`

The system variables related to the <a undefined>aws_key_management</a> encryption plugin can be found [here](/kb/en/aws-key-management-encryption-plugin/#system-variables).

The options elated to the <a undefined>aws_key_management</a> encryption plugin can be found [here](/kb/en/aws-key-management-encryption-plugin/#options).

### Encryption Plugin - `file_key_management`

The system variables related to the [file_key_management](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/key-management-and-encryption-plugins/file-key-management-encryption-plugin/) encryption plugin can be found [here](/kb/en/file-key-management-encryption-plugin/#system-variables).

The options related to the [file_key_management](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/key-management-and-encryption-plugins/file-key-management-encryption-plugin/) encryption plugin can be found [here](/kb/en/file-key-management-encryption-plugin/#options).

## Password Validation Plugins - Options and System Variables

### Password Validation Plugin - `simple_password_check`

The system variables related to the <a undefined>simple_password_check</a> password validation plugin can be found [here](/kb/en/simple_password_check/#system-variables).

The options related to the <a undefined>simple_password_check</a> password validation plugin can be found [here](/kb/en/simple_password_check/#options).

### Password Validation Plugin - `cracklib_password_check`

The system variables related to the <a undefined>cracklib_password_check</a> password validation plugin can be found [here](/kb/en/cracklib_password_check/#system-variables).

The options related to the <a undefined>cracklib_password_check</a> password validation plugin can be found [here](/kb/en/cracklib_password_check/#options).

## Audit Plugins - Options and System Variables

### Audit Plugin - `server_audit`

Options and system variables related to the [server_audit](/columns-storage-engines-and-plugins/plugins/mariadb-audit-plugin/) audit plugin can be found [here](/kb/en/server_audit-system-variables/).

### Audit Plugin - `SQL_ERROR_LOG`

The system variables related to the [SQL_ERROR_LOG](/mariadb-administration/server-monitoring-logs/sql-error-log-plugin/) audit plugin can be found [here](/kb/en/sql-error-log-plugin/#system-variables).

The options related to the [SQL_ERROR_LOG](/mariadb-administration/server-monitoring-logs/sql-error-log-plugin/) audit plugin can be found [here](/kb/en/sql-error-log-plugin/#options).

### Audit Plugin - QUERY_RESPONSE_TIME_AUDIT

The options related to the [QUERY_RESPONSE_TIME_AUDIT](/columns-storage-engines-and-plugins/plugins/other-plugins/query-response-time-plugin/) audit plugin can be found [here](/kb/en/query-response-time-plugin/#options).

## Daemon Plugins - Options and System Variables

### Daemon Plugin - `handlersocket`

The options for the HandlerSocket plugin are all described on the [HandlerSocket Configuration Option](/sql-statements-structure/nosql/handlersocket/handlersocket-configuration-options/) page.

## Information Schema Plugins - Options and System Variables

### Information Schema Plugin - `DISKS`

The options related to the [DISKS](/columns-storage-engines-and-plugins/plugins/other-plugins/disks-plugin/) information schema plugin can be found [here](/kb/en/disks-plugin/#options).

### Information Schema Plugin - `feedback`

The system variables related to the [feedback](/columns-storage-engines-and-plugins/plugins/other-plugins/feedback-plugin/) plugin can be found [here](/kb/en/feedback-plugin/#system-variables).

The options related to the [feedback](/columns-storage-engines-and-plugins/plugins/other-plugins/feedback-plugin/) plugin can be found [here](/kb/en/feedback-plugin/#options).

### Information Schema Plugin - `LOCALES`

The options related to the [LOCALES](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/locales-plugin/) information schema plugin can be found [here](/kb/en/locales-plugin/#options).

### Information Schema Plugin - `METADATA_LOCK_INFO`

The options related to the <a undefined>METADATA_LOCK_INFO</a> information schema plugin can be found [here](/kb/en/metadata_lock_info/#options).

### Information Schema Plugin - `QUERY_CACHE_INFO`

The options related to the [QUERY_CACHE_INFO](/columns-storage-engines-and-plugins/plugins/other-plugins/query-cache-information-plugin/) information schema plugin can be found [here](/kb/en/query-cache-information-plugin/#options).

### Information Schema Plugin - `QUERY_RESPONSE_TIME`

The system variables related to the [QUERY_RESPONSE_TIME](/columns-storage-engines-and-plugins/plugins/other-plugins/query-response-time-plugin/) information schema plugin can be found [here](/kb/en/query-response-time-plugin/#system-variables).

The options related to the [QUERY_RESPONSE_TIME](/columns-storage-engines-and-plugins/plugins/other-plugins/query-response-time-plugin/) information schema plugin can be found [here](/kb/en/query-response-time-plugin/#options).

### Information Schema Plugin - `user_variables`

The options related to the [user_variables](/columns-storage-engines-and-plugins/plugins/other-plugins/user-variables-plugin/) information schema plugin can be found [here](/kb/en/user-variables-plugin/#options).

### Information Schema Plugin - `WSREP_MEMBERSHIP`

The options related to the [WSREP_MEMBERSHIP](/columns-storage-engines-and-plugins/plugins/mariadb-replication-cluster-plugins/wsrep_info-plugin/) information schema plugin can be found [here](/kb/en/wsrep_info-plugin/#options).

### Information Schema Plugin - `WSREP_STATUS`

The options related to the [WSREP_STATUS](/columns-storage-engines-and-plugins/plugins/mariadb-replication-cluster-plugins/wsrep_info-plugin/) information schema plugin can be found [here](/kb/en/wsrep_info-plugin/#options).

## Replication Plugins - Options and System Variables

### Replication Plugin - `rpl_semi_sync_master`

The system variables related to the [rpl_semi_sync_master](/replication/standard-replication/semisynchronous-replication/) replication plugin can be found [here](/kb/en/semisynchronous-replication/#system-variables).

The options related to the [rpl_semi_sync_master](/replication/standard-replication/semisynchronous-replication/) replication plugin can be found [here](/kb/en/semisynchronous-replication/#options).

### Replication Plugin - `rpl_semi_sync_slave`

The system variables related to the [rpl_semi_sync_slave](/replication/standard-replication/semisynchronous-replication/) replication plugin can be found [here](/kb/en/semisynchronous-replication/#system-variables).

The options related to the [rpl_semi_sync_slave](/replication/standard-replication/semisynchronous-replication/) replication plugin can be found [here](/kb/en/semisynchronous-replication/#options).

## Default Values

You can verify the default values for an option by doing:

```sql
mysqld --no-defaults --help --verbose
```