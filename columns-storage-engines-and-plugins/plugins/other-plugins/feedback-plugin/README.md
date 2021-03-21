# Feedback Plugin

The `feedback` plugin is designed to collect and, optionally, upload
configuration and usage information to [MariaDB.org](http://mariadb.org/) or to any other configured URL.

See the [MariaDB User Feedback](http://mariadb.org/feedback_plugin/) page on MariaDB.org to see collected MariaDB usage statistics.

The `feedback` plugin exists in all MariaDB versions.

MariaDB is distributed with this plugin included, but it is not enabled by default.
On Windows, this plugin is part of the server and has a special checkbox in the installer window. Either
way, you need to explicitly install and enable it in order for feedback data to be sent.

## Installing the Plugin

Although the plugin's shared library is distributed with MariaDB by default, the plugin is not actually installed by MariaDB by default. There are two methods that can be used to install the plugin with MariaDB.

The first method can be used to install the plugin without restarting the server. You can install the plugin dynamically by executing [INSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-soname/) or [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin/). For example:

```sql
INSTALL SONAME 'feedback';
```

The second method can be used to tell the server to load the plugin when it starts up. The plugin can be installed this way by providing the <a undefined>--plugin-load</a> or the <a undefined>--plugin-load-add</a> options. This can be specified as a command-line argument to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) or it can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/). For example:

```sql
[mariadb]
...
plugin_load_add = feedback
```

## Uninstalling the Plugin

You can uninstall the plugin dynamically by executing [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname/) or [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin/). For example:

```sql
UNINSTALL SONAME 'feedback';
```

If you installed the plugin by providing the <a undefined>--plugin-load</a> or the <a undefined>--plugin-load-add</a> options in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/), then those options should be removed to prevent the plugin from being loaded the next time the server is restarted.

## Enabling the Plugin

You can enable the plugin by setting the <a undefined>feedback</a> option to `ON` in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/). For example:

```sql
[mariadb]
...
feedback=ON
```

In Windows, the plugin can also be enabled during a new [MSI](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-msi-packages-on-windows/) installation. The MSI GUI installation provides the "Enable feedback plugin" checkbox to enable the plugin. The MSI command-line installation provides the FEEDBACK=1 command-line option to enable the plugin.

See the next section for how to verify the plugin is installed and active and (if needed) install the plugin.

## Verifying the Plugin's Status

To verify whether the `feedback` plugin is installed and enabled, execute the
[SHOW PLUGINS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-plugins/) statement or query the [information_schema.plugins](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/plugins-table-information-schema/) table. For example:

```sql
SELECT plugin_status FROM information_schema.plugins 
  WHERE plugin_name = 'feedback';
```

If that `SELECT` returns no rows, then you still need to [install the plugin](#installing-the-plugin).

When the plugin is installed and enabled, you will see:

```sql
SELECT plugin_status FROM information_schema.plugins 
  WHERE plugin_name = 'feedback';
+---------------+
| plugin_status |
+---------------+
| ACTIVE        |
+---------------+
```

If you see `DISABLED` instead of `ACTIVE`, then you still need to [enable the plugin](#enabling-the-plugin).

## Collecting Data

The `feedback` plugin will collect:

- Certain rows from [SHOW STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-status/) and [SHOW VARIABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-variables/).
- All installed [plugins](/columns-storage-engines-and-plugins/plugins/) and their versions.
- System information such as CPU count, memory, architecture, and OS/linux distribution.
- The <a undefined>feedback_server_uid</a>, which is a SHA1 hash of the MAC address of the first network interface and the TCP port that the server listens on.

The `feedback` plugin creates the <a undefined>FEEDBACK</a> table in the [INFORMATION_SCHEMA](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/) database. To see the data that has been collected by the plugin, you can execute:

```sql
SELECT * FROM information_schema.feedback;
```

Only the contents of this table are sent to the <a undefined>feedback_url</a>.

##### MariaDB starting with [10.0.14](/kb/en/mariadb-10014-release-notes/)

[MariaDB 10.0.14](/kb/en/mariadb-10014-release-notes/) added collation usage statistics. Each collation that has been used by the server
will have a record in "SELECT * FROM information_schema.feedback" output, for example:

```sql
+----------------------------------------+---------------------+
| VARIABLE_NAME                          | VARIABLE_VALUE      |
+----------------------------------------+---------------------+
| Collation used utf8_unicode_ci         | 10                  |
| Collation used latin1_general_ci       | 20                  |
+----------------------------------------+---------------------+
```

Collations that have not been used will not be included into the result.

## Sending Data

The `feedback` plugin sends the data using a `POST` request to any URL or a list of URLs
that you specify by setting the <a undefined>feedback_url</a> system variable. By default, this is set to the following URL:

- [https://mariadb.org/feedback_plugin/post](https://mariadb.org/feedback_plugin/post)

Both HTTP and HTTPS protocols are supported.

If HTTP traffic requires a proxy in your environment, then you can specify the proxy by setting the <a undefined>feedback_http_proxy</a> system variable.

If the <a undefined>feedback_url</a> system variable is not set to an empty string, then the
plugin will automatically send a report to all URLs in the list a few minutes after the server starts up and then once a week after that.

If the <a undefined>feedback_url</a> system variable is set to an empty string, then the
plugin will <strong>not</strong> automatically send any data. This may be necessary if outbound HTTP communication from your database server is not permitted. In this case, you can still upload the data manually, if you'd like.

First, generate the report file with the MariaDB command-line [mysq](/clients-utilities/mysql-client/mysql-command-line-client/)l client:

```sql
$ mysql -e 'select * from information_schema.feedback' > report.txt
```

Then you can upload the generated `report.txt` [here](https://mariadb.org/feedback_plugin/post) using your web browser.

Or you can do it from the command line with tools such as <a undefined>curl</a>. For example:

```sql
$ curl -F data=@report.txt https://mariadb.org/feedback_plugin/post
```

Manual uploading allows you to be absolutely sure that we receive only the data shown in the [INFORMATION_SCHEMA.FEEDBACK](/kb/en/information-schema-feedback-table/) table and that no private or sensitive information is being sent.

## Versions

<table><tbody><tr><th>Version</th><th>Status</th><th>Introduced</th></tr>
<tr><td>1.1</td><td>Stable</td><td><a href="/kb/en/mariadb-10010-release-notes/">MariaDB 10.0.10</a></td></tr>
<tr><td>1.1</td><td>Beta</td><td><a href="/kb/en/mariadb-5520-release-notes/">MariaDB 5.5.20</a>, <a href="/kb/en/mariadb-533-release-notes/">MariaDB 5.3.3</a></td></tr>
</tbody></table>

## System Variables

### `feedback_http_proxy`

- <strong>Description:</strong> Proxy server for use when http calls cannot be made, such as in a firewall environment. The format is `host:port`.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--feedback-http=proxy=value</code>
- <strong>Read-only:</strong> Yes
- <strong>Data Type:</strong> string
- <strong>Default Value:</strong> `''` (empty)
- <strong>Introduced:</strong> [MariaDB 5.5.48](/kb/en/mariadb-5548-release-notes/), [MariaDB 10.1.8](/kb/en/mariadb-1018-release-notes/)

---

### `feedback_send_retry_wait`

- <strong>Description:</strong> Time in seconds before retrying if the plugin failed to send the data for any reason.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--feedback-send-retry-wait=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> numeric
- <strong>Default Value:</strong> `60`
- <strong>Valid Values:</strong> `1` to `86400`

---

### `feedback_send_timeout`

- <strong>Description:</strong> An attempt to send the data times out and fails after this many seconds.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--feedback-send-timeout=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> numeric
- <strong>Default Value:</strong> `60`
- <strong>Valid Values:</strong> `1` to `86400`

---

### `feedback_server_uid`

- <strong>Description:</strong> Automatically calculated server unique id hash.
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> string

---

### `feedback_url`

- <strong>Description:</strong> URL to which the data is sent. More than one URL, separated by spaces, can be specified. Set it to an empty string to disable data sending.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--feedback-url=url</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> string
- <strong>Default Value:</strong> <a undefined>https://mariadb.org/feedback_plugin/post</a>

---

### `feedback_user_info`

- <strong>Description:</strong> The value of this option is not used by the plugin, but it is included in the feedback data. It can be used to add any user-specified string to the report. This could be used to help to identify it. For example, a support contract number, or a computer name (if you collect reports internally by specifying your own `feedback-url`).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--feedback-user-info=string</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> string
- <strong>Default Value:</strong> Empty string

---

## Options

### `feedback`

- <strong>Description:</strong> Controls how the server should treat the plugin when the server starts up.
<ul start="1"><li>Valid values are:
<ul start="1"><li>`OFF` - Disables the plugin without removing it from the <a undefined>mysql.plugins</a> table.
</li><li>`ON` - Enables the plugin. If the plugin cannot be initialized, then the server will still continue starting up, but the plugin will be disabled.
</li><li>`FORCE` - Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error.
</li><li>`FORCE_PLUS_PERMANENT` - Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error. In addition, the plugin cannot be uninstalled with [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname/) or [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin/) while the server is running.
</li></ul>
</li><li>See [Plugin Overview: Configuring Plugin Activation at Server Startup](/kb/en/plugin-overview/#configuring-plugin-activation-at-server-startup) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--feedback=value</code>
- <strong>Data Type:</strong> `enumerated`
- <strong>Default Value:</strong> `ON`
- <strong>Valid Values:</strong> `OFF`, `ON`, `FORCE`, `FORCE_PLUS_PERMANENT`

---