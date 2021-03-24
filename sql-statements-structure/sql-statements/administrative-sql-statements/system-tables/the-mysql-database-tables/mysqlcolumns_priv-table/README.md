# mysql.columns_priv Table

The `mysql.columns_priv` table contains information about column-level privileges. The table can be queried and although it is possible to directly update it, it is best to use [GRANT](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) for setting privileges.

Note that the MariaDB privileges occur at many levels. A user may be granted a privilege at the column level, but may still not have permission on a table level, for example. See [privileges](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) for a more complete view of the MariaDB privilege system.

The [INFORMATION_SCHEMA.COLUMN_PRIVILEGES](/kb/en/information-schema-column_privileges-table/) table derives its contents from `mysql.columns_priv`.

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, this table uses the [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/) storage engine.

##### MariaDB until [10.3](/kb/en/what-is-mariadb-103/)

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and before, this table uses the [MyISAM](/columns-storage-engines-and-plugins/storage-engines/myisam-storage-engine/) storage engine.

The `mysql.columns_priv` table contains the following fields:

<table><tbody><tr><th>Field</th><th>Type</th><th>Null</th><th>Key</th><th>Default</th><th>Description</th></tr>
<tr><td><code>Host</code></td><td><code>char(60)</code></td><td>NO</td><td>PRI</td><td></td><td>Host (together with <code>User</code>, <code>Db</code> , <code>Table_name</code> and<code>Column_name</code> makes up the unique identifier for this record.</td></tr>
<tr><td><code>Db</code></td><td><code>char(64)</code></td><td>NO</td><td>PRI</td><td></td><td>Database name (together with <code>User</code>, <code>Host</code> , <code>Table_name</code> and<code>Column_name</code> makes up the unique identifier for this record.</td></tr>
<tr><td><code>User</code></td><td><code>char(80)</code></td><td>NO</td><td>PRI</td><td></td><td>User (together with <code>Host</code>, <code>Db</code> , <code>Table_name</code> and<code>Column_name</code> makes up the unique identifier for this record.</td></tr>
<tr><td><code>Table_name</code></td><td><code>char(64)</code></td><td>NO</td><td>PRI</td><td></td><td>Table name (together with <code>User</code>, <code>Db</code> , <code>Host</code> and<code>Column_name</code> makes up the unique identifier for this record.</td></tr>
<tr><td><code>Column_name</code></td><td><code>char(64)</code></td><td>NO</td><td>PRI</td><td></td><td>Column name (together with <code>User</code>, <code>Db</code> , <code>Table_name</code> and<code>Host</code> makes up the unique identifier for this record.</td></tr>
<tr><td><code>Timestamp</code></td><td><code>timestamp</code></td><td>NO</td><td></td><td><code>CURRENT_TIMESTAMP</code></td><td></td></tr>
<tr><td><code>Column_priv</code></td><td><code>set('Select', 'Insert', 'Update', 'References')</code></td><td>NO</td><td></td><td></td><td>The privilege type. See <a href="/kb/en/grant/#column-privileges">Column Privileges</a> for details.</td></tr>
</tbody></table>

The [Acl_column_grants](/kb/en/server-status-variables/#acl_column_grants) status variable, added in [MariaDB 10.1.4](/kb/en/mariadb-1014-release-notes/), indicates how many rows the `mysql.columns_priv` table contains.