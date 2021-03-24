# mysql.host Table

### Usage

Until [MariaDB 5.5](/kb/en/what-is-mariadb-55/), the `mysql.host` table contained information about hosts and their related privileges. When determining permissions, if a matching record in the [mysql.db table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqldb-table/) had a blank host value, the mysql.host table would be examined.

This table is not effected by an [GRANT](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) statements, and had to be updated manually.

See [privileges](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) for a more complete view of the MariaDB privilege system.

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

In [MariaDB 10.0](/kb/en/what-is-mariadb-100/) and later, this table is no longer used.

##### MariaDB until [10.3](/kb/en/what-is-mariadb-103/)

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and before, this table was created the [MyISAM](/columns-storage-engines-and-plugins/storage-engines/myisam-storage-engine/) storage engine.

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, this table is no longer created. However if the table is created it will be used.

### Table fields

The `mysql.host` table contains the following fields:

<table><tbody><tr><th>Field</th><th>Type</th><th>Null</th><th>Key</th><th>Default</th><th>Description</th></tr>
<tr><td><code>Host</code></td><td><code>char(60)</code></td><td>NO</td><td>PRI</td><td></td><td>Host (together with <code>Db</code> makes up the unique identifier for this record.</td></tr>
<tr><td><code>Db</code></td><td><code>char(64)</code></td><td>NO</td><td>PRI</td><td></td><td>Database (together with <code>Host</code> makes up the unique identifier for this record.</td></tr>
<tr><td><code>Select_priv</code></td><td><code>enum('N','Y')</code></td><td>NO</td><td></td><td>N</td><td>Can perform <a href="/kb/en/select/">SELECT</a> statements.</td></tr>
<tr><td><code>Insert_priv</code></td><td><code>enum('N','Y')</code></td><td>NO</td><td></td><td>N</td><td>Can perform <a href="/kb/en/insert/">INSERT</a> statements.</td></tr>
<tr><td><code>Update_priv</code></td><td><code>enum('N','Y')</code></td><td>NO</td><td></td><td>N</td><td>Can perform <a href="/kb/en/update/">UPDATE</a> statements.</td></tr>
<tr><td><code>Delete_priv</code></td><td><code>enum('N','Y')</code></td><td>NO</td><td></td><td>N</td><td>Can perform <a href="/kb/en/delete/">DELETE</a> statements.</td></tr>
<tr><td><code>Create_priv</code></td><td><code>enum('N','Y')</code></td><td>NO</td><td></td><td>N</td><td>Can <a href="/kb/en/create-table/">CREATE TABLEs</a>.</td></tr>
<tr><td><code>Drop_priv</code></td><td><code>enum('N','Y')</code></td><td>NO</td><td></td><td>N</td><td>Can <a href="/kb/en/drop-database/">DROP DATABASEs</a> or <a href="/kb/en/drop-table/">DROP TABLEs</a>.</td></tr>
<tr><td><code>Grant_priv</code></td><td><code>enum('N','Y')</code></td><td>NO</td><td></td><td>N</td><td>User can <a href="/kb/en/grant/">grant</a> privileges they possess.</td></tr>
<tr><td><code>References_priv</code></td><td><code>enum('N','Y')</code></td><td>NO</td><td></td><td>N</td><td>Unused</td></tr>
<tr><td><code>Index_priv</code></td><td><code>enum('N','Y')</code></td><td>NO</td><td></td><td>N</td><td>Can create an index on a table using the <a href="/kb/en/create-index/">CREATE INDEX</a> statement. Without the <code>INDEX</code> privilege, user can still create indexes when creating a table using the <a href="/kb/en/create-table/">CREATE TABLE</a> statement if the user has have the <code>CREATE</code> privilege, and user can create indexes using the <a href="/kb/en/alter-table/">ALTER TABLE</a> statement if they have the <code>ALTER</code> privilege.</td></tr>
<tr><td><code>Alter_priv</code></td><td><code>enum('N','Y')</code></td><td>NO</td><td></td><td>N</td><td>Can perform <a href="/kb/en/alter-table/">ALTER TABLE</a> statements.</td></tr>
<tr><td><code>Create_tmp_table_priv</code></td><td><code>enum('N','Y')</code></td><td>NO</td><td></td><td>N</td><td>Can create temporary tables with the <a href="/kb/en/create-table/">CREATE TEMPORARY TABLE</a> statement.</td></tr>
<tr><td><code>Lock_tables_priv</code></td><td><code>enum('N','Y')</code></td><td>NO</td><td></td><td>N</td><td>Acquire explicit locks using the <a href="/kb/en/transactions-lock/">LOCK TABLES</a> statement; user also needs to have the <code>SELECT</code> privilege on a table in order to lock it.</td></tr>
<tr><td><code>Create_view_priv</code></td><td><code>enum('N','Y')</code></td><td>NO</td><td></td><td>N</td><td>Can create a view using the <a href="/kb/en/create-view/">CREATE_VIEW</a> statement.</td></tr>
<tr><td><code>Show_view_priv</code></td><td><code>enum('N','Y')</code></td><td>NO</td><td></td><td>N</td><td>Can show the <a href="/kb/en/create-view/">CREATE VIEW</a> statement to create a view using the <a href="/kb/en/show-create-view/">SHOW CREATE VIEW</a> statement.</td></tr>
<tr><td><code>Create_routine_priv</code></td><td><code>enum('N','Y')</code></td><td>NO</td><td></td><td>N</td><td>Can create stored programs using the <a href="/kb/en/create-procedure/">CREATE PROCEDURE</a> and <a href="/kb/en/create-function/">CREATE FUNCTION</a> statements.</td></tr>
<tr><td><code>Alter_routine_priv</code></td><td><code>enum('N','Y')</code></td><td>NO</td><td></td><td>N</td><td>Can change the characteristics of a stored function using the <a href="/kb/en/alter-function/">ALTER FUNCTION</a> statement.</td></tr>
<tr><td><code>Execute_priv</code></td><td><code>enum('N','Y')</code></td><td>NO</td><td></td><td>N</td><td>Can execute <a href="/kb/en/stored-procedures/">stored procedure</a> or functions.</td></tr>
<tr><td><code>Trigger_priv</code></td><td><code>enum('N','Y')</code></td><td>NO</td><td></td><td>N</td><td>Can execute <a href="/kb/en/triggers/">triggers</a> associated with tables the user updates, execute the <a href="/kb/en/create-trigger/">CREATE TRIGGER</a> and <a href="/kb/en/drop-trigger/">DROP TRIGGER</a> statements.</td></tr>
</tbody></table>

### How to create

If you need the functionality to only allow access to your database from a given set of hosts, you can create the host table with the following command:

```sql
CREATE TABLE IF NOT EXISTS mysql.host (Host char(60) binary DEFAULT '' NOT NULL,
Db char(64) binary DEFAULT '' NOT NULL,
Select_priv enum('N','Y') COLLATE utf8_general_ci DEFAULT 'N' NOT NULL,
Insert_priv enum('N','Y') COLLATE utf8_general_ci DEFAULT 'N' NOT NULL,
Update_priv enum('N','Y') COLLATE utf8_general_ci DEFAULT 'N' NOT NULL,
Delete_priv enum('N','Y') COLLATE utf8_general_ci DEFAULT 'N' NOT NULL,
Create_priv enum('N','Y') COLLATE utf8_general_ci DEFAULT 'N' NOT NULL,
Drop_priv enum('N','Y') COLLATE utf8_general_ci DEFAULT 'N' NOT NULL,
Grant_priv enum('N','Y') COLLATE utf8_general_ci DEFAULT 'N' NOT NULL,
References_priv enum('N','Y') COLLATE utf8_general_ci DEFAULT 'N' NOT NULL,
Index_priv enum('N','Y') COLLATE utf8_general_ci DEFAULT 'N' NOT NULL,
Alter_priv enum('N','Y') COLLATE utf8_general_ci DEFAULT 'N' NOT NULL,
Create_tmp_table_priv enum('N','Y') COLLATE utf8_general_ci DEFAULT 'N' NOT NULL,
Lock_tables_priv enum('N','Y') COLLATE utf8_general_ci DEFAULT 'N' NOT NULL,
Create_view_priv enum('N','Y') COLLATE utf8_general_ci DEFAULT 'N' NOT NULL,
Show_view_priv enum('N','Y') COLLATE utf8_general_ci DEFAULT 'N' NOT NULL,
Create_routine_priv enum('N','Y') COLLATE utf8_general_ci DEFAULT 'N' NOT NULL,
Alter_routine_priv enum('N','Y') COLLATE utf8_general_ci DEFAULT 'N' NOT NULL,
Execute_priv enum('N','Y') COLLATE utf8_general_ci DEFAULT 'N' NOT NULL,
Trigger_priv enum('N','Y') COLLATE utf8_general_ci DEFAULT 'N' NOT NULL,
PRIMARY KEY /*Host*/ (Host,Db) )
engine=MyISAM CHARACTER SET utf8 COLLATE utf8_bin
comment='Host privileges;  Merged with database privileges';
```