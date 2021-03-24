# mysql.procs_priv Table

The `mysql.procs_priv` table contains information about [stored procedure](/programming-customizing-mariadb/stored-routines/stored-procedures/) and [stored function](/programming-customizing-mariadb/stored-routines/stored-functions/) privileges. See [CREATE PROCEDURE](/programming-customizing-mariadb/stored-routines/stored-procedures/create-procedure/) and [CREATE FUNCTION](/sql-statements-structure/sql-statements/data-definition/create/create-function/) on creating these.

The [INFORMATION_SCHEMA.ROUTINES](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-routines-table/) table derives its contents from `mysql.procs_priv`.

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, this table uses the [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/) storage engine.

##### MariaDB until [10.3](/kb/en/what-is-mariadb-103/)

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and before, this table uses the [MyISAM](/columns-storage-engines-and-plugins/storage-engines/myisam-storage-engine/) storage engine.

The `mysql.procs_priv` table contains the following fields:

<table><tbody><tr><th>Field</th><th>Type</th><th>Null</th><th>Key</th><th>Default</th><th>Description</th></tr>
<tr><td><code>Host</code></td><td><code>char(60)</code></td><td>NO</td><td>PRI</td><td></td><td>Host (together with <code>Db</code>, <code>User</code>, <code>Routine_name</code> and <code>Routine_type</code> makes up the unique identifier for this record).</td></tr>
<tr><td><code>Db</code></td><td><code>char(64)</code></td><td>NO</td><td>PRI</td><td></td><td>Database (together with <code>Host</code>, <code>User</code>, <code>Routine_name</code> and <code>Routine_type</code> makes up the unique identifier for this record).</td></tr>
<tr><td><code>User</code></td><td><code>char(80)</code></td><td>NO</td><td>PRI</td><td></td><td>User (together with <code>Host</code>, <code>Db</code>, <code>Routine_name</code> and <code>Routine_type</code> makes up the unique identifier for this record).</td></tr>
<tr><td><code>Routine_name</code></td><td><code>char(64)</code></td><td>NO</td><td>PRI</td><td></td><td>Routine_name (together with <code>Host</code>, <code>Db</code> <code>User</code> and <code>Routine_type</code> makes up the unique identifier for this record).</td></tr>
<tr><td><code>Routine_type</code></td><td><code>enum('FUNCTION','PROCEDURE', 'PACKAGE', 'PACKAGE BODY')</code></td><td>NO</td><td>PRI</td><td><code>NULL</code></td><td>Whether the routine is a <a href="/kb/en/stored-procedures/">stored procedure</a>, <a href="/kb/en/stored-functions/">stored function</a>, or, from <a href="/kb/en/mariadb-1035-release-notes/">MariaDB 10.3.5</a>, a <a href="/kb/en/create-package/">package</a> or <a href="/kb/en/create-package-body/">package body</a>.</td></tr>
<tr><td><code>Grantor</code></td><td><code>char(141)</code></td><td>NO</td><td>MUL</td><td></td><td></td></tr>
<tr><td><code>Proc_priv</code></td><td><code>set('Execute','Alter Routine','Grant')</code></td><td>NO</td><td></td><td></td><td>The routine privilege. See <a href="/kb/en/grant/#function-privileges">Function Privileges</a> and <a href="/kb/en/grant/#procedure-privileges">Procedure Privileges</a> for details.</td></tr>
<tr><td><code>Timestamp</code></td><td><code>timestamp</code></td><td>NO</td><td></td><td><code>CURRENT_TIMESTAMP</code></td><td></td></tr>
</tbody></table>

The [Acl_function_grants](/kb/en/server-status-variables/#acl_function_grants) status variable, added in [MariaDB 10.1.4](/kb/en/mariadb-1014-release-notes/), indicates how many rows the `mysql.columns_priv` table contains with the `FUNCTION` routine type.

The [Acl_procedure_grants](/kb/en/server-status-variables/#acl_procedure_grants) status variable, added in [MariaDB 10.1.4](/kb/en/mariadb-1014-release-notes/), indicates how many rows the `mysql.columns_priv` table contains with the `PROCEDURE` routine type.