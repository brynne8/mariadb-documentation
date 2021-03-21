# mysql.roles_mapping Table

##### MariaDB starting with [10.0.5](/kb/en/mariadb-1005-release-notes/)

[Roles](/mariadb-administration/user-server-security/user-account-management/roles) were introduced in [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/).

The `mysql.roles_mapping` table contains information about mariaDB [roles](/mariadb-administration/user-server-security/user-account-management/roles).

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, this table uses the [Aria](/columns-storage-engines-and-plugins/storage-engines/aria) storage engine.

##### MariaDB until [10.3](/kb/en/what-is-mariadb-103/)

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and before, this table uses the [MyISAM](/columns-storage-engines-and-plugins/storage-engines/myisam-storage-engine) storage engine.

The `mysql.roles_mapping` table contains the following fields:

<table><tbody><tr><th>Field</th><th>Type</th><th>Null</th><th>Key</th><th>Default</th><th>Description</th></tr>
<tr><td><code>Host</code></td><td><code>char(60)</code></td><td>NO</td><td>PRI</td><td></td><td>Host (together with <code>User</code> and <code>Role</code> makes up the unique identifier for this record.</td></tr>
<tr><td><code>User</code></td><td><code>char(80)</code></td><td>NO</td><td>PRI</td><td></td><td>User (together with <code>Host</code> and <code>Role</code> makes up the unique identifier for this record.</td></tr>
<tr><td><code>Role</code></td><td><code>char(80)</code></td><td>NO</td><td>PRI</td><td></td><td>Role (together with <code>Host</code> and <code>User</code> makes up the unique identifier for this record.</td></tr>
<tr><td><code>Admin_option</code></td><td><code>enum('N','Y')</code></td><td>NO</td><td></td><td>N</td><td>Whether the role can be granted (see the <a href="/kb/en/create-role/">CREATE ROLE</a> <code>WITH ADMIN</code> clause).</td></tr>
</tbody></table>

The [Acl_role_grants](/kb/en/server-status-variables/#acl_role_grants) status variable, added in [MariaDB 10.1.4](/kb/en/mariadb-1014-release-notes/), indicates how many rows the `mysql.roles_mapping` table contains.