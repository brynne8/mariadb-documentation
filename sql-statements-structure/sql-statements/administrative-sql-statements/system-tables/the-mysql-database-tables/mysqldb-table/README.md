# mysql.db Table

The `mysql.db` table contains information about database-level privileges. The table can be queried and although it is possible to directly update it, it is best to use [GRANT](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) for setting privileges.

Note that the MariaDB privileges occur at many levels. A user may not be granted a privilege at the database level, but may still have permission on a table level, for example. See [privileges](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) for a more complete view of the MariaDB privilege system.

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, this table uses the [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/) storage engine.

##### MariaDB until [10.3](/kb/en/what-is-mariadb-103/)

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and before, this table uses the [MyISAM](/columns-storage-engines-and-plugins/storage-engines/myisam-storage-engine/) storage engine.

The `mysql.db` table contains the following fields:

<table><tbody><tr><th>Field</th><th>Type</th><th>Null</th><th>Key</th><th>Default</th><th>Description</th><th>Introduced</th></tr>
<tr><td><code>Host</code></td><td><code>char(60)</code></td><td>NO</td><td>PRI</td><td></td><td>Host (together with <code>User</code> and <code>Db</code> makes up the unique identifier for this record. Until <a href="/kb/en/what-is-mariadb-55/">MariaDB 5.5</a>, if the host field was blank, the corresponding record in the <a href="/kb/en/mysqlhost-table/">mysql.host</a> table would be examined. From <a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a>, a blank host field is the same as the <code>%</code> wildcard.</td><td></td></tr>
<tr><td><code>Db</code></td><td><code>char(64)</code></td><td>NO</td><td>PRI</td><td></td><td>Database (together with <code>User</code> and <code>Host</code> makes up the unique identifier for this record.</td><td></td></tr>
<tr><td><code>User</code></td><td><code>char(80)</code></td><td>NO</td><td>PRI</td><td></td><td>User (together with <code>Host</code> and <code>Db</code> makes up the unique identifier for this record.</td><td></td></tr>
<tr><td><code>Select_priv</code></td><td><code>enum('N','Y')</code></td><td>NO</td><td></td><td>N</td><td>Can perform <a href="/kb/en/select/">SELECT</a> statements.</td><td></td></tr>
<tr><td><code>Insert_priv</code></td><td><code>enum('N','Y')</code></td><td>NO</td><td></td><td>N</td><td>Can perform <a href="/kb/en/insert/">INSERT</a> statements.</td><td></td></tr>
<tr><td><code>Update_priv</code></td><td><code>enum('N','Y')</code></td><td>NO</td><td></td><td>N</td><td>Can perform <a href="/kb/en/update/">UPDATE</a> statements.</td><td></td></tr>
<tr><td><code>Delete_priv</code></td><td><code>enum('N','Y')</code></td><td>NO</td><td></td><td>N</td><td>Can perform <a href="/kb/en/delete/">DELETE</a> statements.</td><td></td></tr>
<tr><td><code>Create_priv</code></td><td><code>enum('N','Y')</code></td><td>NO</td><td></td><td>N</td><td>Can <a href="/kb/en/create-table/">CREATE TABLE's</a>.</td><td></td></tr>
<tr><td><code>Drop_priv</code></td><td><code>enum('N','Y')</code></td><td>NO</td><td></td><td>N</td><td>Can <a href="/kb/en/drop-database/">DROP DATABASE's</a> or <a href="/kb/en/drop-table/">DROP TABLE's</a>.</td><td></td></tr>
<tr><td><code>Grant_priv</code></td><td><code>enum('N','Y')</code></td><td>NO</td><td></td><td>N</td><td>User can <a href="/kb/en/grant/">grant</a> privileges they possess.</td><td></td></tr>
<tr><td><code>References_priv</code></td><td><code>enum('N','Y')</code></td><td>NO</td><td></td><td>N</td><td>Unused</td><td></td></tr>
<tr><td><code>Index_priv</code></td><td><code>enum('N','Y')</code></td><td>NO</td><td></td><td>N</td><td>Can create an index on a table using the <a href="/kb/en/create-index/">CREATE INDEX</a> statement. Without the <code>INDEX</code> privilege, user can still create indexes when creating a table using the <a href="/kb/en/create-table/">CREATE TABLE</a> statement if the user has have the <code>CREATE</code> privilege, and user can create indexes using the <a href="/kb/en/alter-table/">ALTER TABLE</a> statement if they have the <code>ALTER</code> privilege.</td><td></td></tr>
<tr><td><code>Alter_priv</code></td><td><code>enum('N','Y')</code></td><td>NO</td><td></td><td>N</td><td>Can perform <a href="/kb/en/alter-table/">ALTER TABLE</a> statements.</td><td></td></tr>
<tr><td><code>Create_tmp_table_priv</code></td><td><code>enum('N','Y')</code></td><td>NO</td><td></td><td>N</td><td>Can create temporary tables with the <a href="/kb/en/create-table/">CREATE TEMPORARY TABLE</a> statement.</td><td></td></tr>
<tr><td><code>Lock_tables_priv</code></td><td><code>enum('N','Y')</code></td><td>NO</td><td></td><td>N</td><td>Acquire explicit locks using the <a href="/kb/en/transactions-lock/">LOCK TABLES</a> statement; user also needs to have the <code>SELECT</code> privilege on a table in order to lock it.</td><td></td></tr>
<tr><td><code>Create_view_priv</code></td><td><code>enum('N','Y')</code></td><td>NO</td><td></td><td>N</td><td>Can create a view using the <a href="/kb/en/create-view/">CREATE_VIEW</a> statement.</td><td></td></tr>
<tr><td><code>Show_view_priv</code></td><td><code>enum('N','Y')</code></td><td>NO</td><td></td><td>N</td><td>Can show the <a href="/kb/en/create-view/">CREATE VIEW</a> statement to create a view using the <a href="/kb/en/show-create-view/">SHOW CREATE VIEW</a> statement.</td><td></td></tr>
<tr><td><code>Create_routine_priv</code></td><td><code>enum('N','Y')</code></td><td>NO</td><td></td><td>N</td><td>Can create stored programs using the <a href="/kb/en/create-procedure/">CREATE PROCEDURE</a> and <a href="/kb/en/create-function/">CREATE FUNCTION</a> statements.</td><td></td></tr>
<tr><td><code>Alter_routine_priv</code></td><td><code>enum('N','Y')</code></td><td>NO</td><td></td><td>N</td><td>Can change the characteristics of a stored function using the <a href="/kb/en/alter-function/">ALTER FUNCTION</a> statement.</td><td></td></tr>
<tr><td><code>Execute_priv</code></td><td><code>enum('N','Y')</code></td><td>NO</td><td></td><td>N</td><td>Can execute <a href="/kb/en/stored-procedures/">stored procedure</a> or functions.</td><td></td></tr>
<tr><td><code>Event_priv</code></td><td><code>enum('N','Y')</code></td><td>NO</td><td></td><td>N</td><td>Create, drop and alter <a href="/kb/en/stored-programs-and-views-events/">events</a>.</td><td></td></tr>
<tr><td><code>Trigger_priv</code></td><td><code>enum('N','Y')</code></td><td>NO</td><td></td><td>N</td><td>Can execute <a href="/kb/en/triggers/">triggers</a> associated with tables the user updates, execute the <a href="/kb/en/create-trigger/">CREATE TRIGGER</a> and <a href="/kb/en/drop-trigger/">DROP TRIGGER</a> statements.</td><td></td></tr>
<tr><td><code>Delete_history_priv</code></td><td><code>enum('N','Y')</code></td><td>NO</td><td></td><td>N</td><td>Can delete rows created through <a href="/kb/en/system-versioned-tables/">system versioning</a>.</td><td><a href="/kb/en/mariadb-1035-release-notes/">MariaDB 10.3.5</a></td></tr>
</tbody></table>

The [Acl_database_grants](/kb/en/server-status-variables/#acl_database_grants) status variable, added in [MariaDB 10.1.4](/kb/en/mariadb-1014-release-notes/), indicates how many rows the `mysql.db` table contains.