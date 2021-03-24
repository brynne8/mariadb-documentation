# mysql.proxies_priv Table

The `mysql.proxies_priv` table contains information about proxy privileges. The table can be queried and although it is possible to directly update it, it is best to use [GRANT](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) for setting privileges.

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, this table uses the [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/) storage engine.

##### MariaDB until [10.3](/kb/en/what-is-mariadb-103/)

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and before, this table uses the [MyISAM](/columns-storage-engines-and-plugins/storage-engines/myisam-storage-engine/) storage engine.

The `mysql.proxies_priv` table contains the following fields:

<table><tbody><tr><th>Field</th><th>Type</th><th>Null</th><th>Key</th><th>Default</th><th>Description</th></tr>
<tr><td><code>Host</code></td><td><code>char(60)</code></td><td>NO</td><td>PRI</td><td></td><td></td></tr>
<tr><td><code>User</code></td><td><code>char(80)</code></td><td>NO</td><td>PRI</td><td></td><td></td></tr>
<tr><td><code>Proxied_host</code></td><td><code>char(60)</code></td><td>NO</td><td>PRI</td><td></td><td></td></tr>
<tr><td><code>Proxied_user</code></td><td><code>char(80)</code></td><td>NO</td><td>PRI</td><td></td><td></td></tr>
<tr><td><code>With_grant</code></td><td><code>tinyint(1)</code></td><td>NO</td><td></td><td>0</td><td></td></tr>
<tr><td><code>Grantor</code></td><td><code>char(141)</code></td><td>NO</td><td>MUL</td><td></td><td></td></tr>
<tr><td><code>Timestamp</code></td><td><code>timestamp</code></td><td>NO</td><td></td><td><code>CURRENT_TIMESTAMP</code></td><td></td></tr>
</tbody></table>

The [Acl_proxy_users](/kb/en/server-status-variables/#acl_proxy_users) status variable, added in [MariaDB 10.1.4](/kb/en/mariadb-1014-release-notes/), indicates how many rows the `mysql.proxies_priv` table contains.