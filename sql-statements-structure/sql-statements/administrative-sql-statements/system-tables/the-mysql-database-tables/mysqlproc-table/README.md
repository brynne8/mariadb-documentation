# mysql.proc Table

The `mysql.proc` table contains information about [stored procedures](/programming-customizing-mariadb/stored-routines/stored-procedures/) and [stored functions](/programming-customizing-mariadb/stored-routines/stored-functions/). It contains similar information to that stored in the [INFORMATION SCHEMA.ROUTINES](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-routines-table/) table.

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, this table uses the [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/) storage engine.

##### MariaDB until [10.3](/kb/en/what-is-mariadb-103/)

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and before, this table uses the [MyISAM](/columns-storage-engines-and-plugins/storage-engines/myisam-storage-engine/) storage engine.

The `mysql.proc` table contains the following fields:

<table><tbody><tr><th>Field</th><th>Type</th><th>Null</th><th>Key</th><th>Default</th><th>Description</th></tr>
<tr><td><code>db</code></td><td><code>char(64)</code></td><td>NO</td><td>PRI</td><td></td><td>Database name.</td></tr>
<tr><td><code>name</code></td><td><code>char(64)</code></td><td>NO</td><td>PRI</td><td></td><td>Routine name.</td></tr>
<tr><td><code>type</code></td><td><code>enum('FUNCTION','PROCEDURE','PACKAGE', 'PACKAGE BODY')</code></td><td>NO</td><td>PRI</td><td><code>NULL</code></td><td>Whether <a href="/kb/en/stored-procedures/">stored procedure</a>, <a href="/kb/en/stored-functions/">stored function</a> or, from <a href="/kb/en/mariadb-1035-release-notes/">MariaDB 10.3.5</a>, a <a href="/kb/en/create-package/">package</a> or <a href="/kb/en/create-package-body/">package body</a>.</td></tr>
<tr><td><code>specific_name</code></td><td><code>char(64)</code></td><td>NO</td><td></td><td></td><td></td></tr>
<tr><td><code>language</code></td><td><code>enum('SQL')</code></td><td>NO</td><td></td><td>SQL</td><td>Always <code>SQL</code>.</td></tr>
<tr><td><code>sql_data_access</code></td><td><code>enum('CONTAINS_SQL', 'NO_SQL', 'READS_SQL_DATA', 'MODIFIES_SQL_DATA')</code></td><td>NO</td><td></td><td><code>CONTAINS_SQL</code></td><td></td></tr>
<tr><td><code>is_deterministic</code></td><td><code>enum('YES','NO')</code></td><td>NO</td><td></td><td>NO</td><td>Whether the routine is deterministic (can produce only one result for a given list of parameters) or not.</td></tr>
<tr><td><code>security_type</code></td><td><code>enum('INVOKER','DEFINER')</code></td><td>NO</td><td></td><td><code>DEFINER</code></td><td><code>INVOKER</code> or <code>DEFINER</code>. Indicates which user's privileges apply to this routine.</td></tr>
<tr><td><code>param_list</code></td><td><code>blob</code></td><td>NO</td><td></td><td><code>NULL</code></td><td>List of parameters.</td></tr>
<tr><td><code>returns</code></td><td><code>longblob</code></td><td>NO</td><td></td><td><code>NULL</code></td><td>What the routine returns.</td></tr>
<tr><td><code>body</code></td><td><code>longblob</code></td><td>NO</td><td></td><td><code>NULL</code></td><td>Definition of the routine.</td></tr>
<tr><td><code>definer</code></td><td><code>char(141)</code></td><td>NO</td><td></td><td></td><td>If the <code>security_type</code> is <code>DEFINER</code>, this value indicates which user defined this routine.</td></tr>
<tr><td><code>created</code></td><td><code>timestamp</code></td><td>NO</td><td></td><td><code>CURRENT_TIMESTAMP</code></td><td>Date and time the routine was created.</td></tr>
<tr><td><code>modified</code></td><td><code>timestamp</code></td><td>NO</td><td></td><td>0000-00-00 00:00:00</td><td>Date and time the routine was modified.</td></tr>
<tr><td><code>sql_mode</code></td><td><code>set('REAL_AS_FLOAT', 'PIPES_AS_CONCAT', 'ANSI_QUOTES', 'IGNORE_SPACE', 'IGNORE_BAD_TABLE_OPTIONS', 'ONLY_FULL_GROUP_BY', 'NO_UNSIGNED_SUBTRACTION', 'NO_DIR_IN_CREATE', 'POSTGRESQL', 'ORACLE', 'MSSQL', 'DB2', 'MAXDB', 'NO_KEY_OPTIONS', 'NO_TABLE_OPTIONS', 'NO_FIELD_OPTIONS', 'MYSQL323', 'MYSQL40', 'ANSI', 'NO_AUTO_VALUE_ON_ZERO', 'NO_BACKSLASH_ESCAPES', 'STRICT_TRANS_TABLES', 'STRICT_ALL_TABLES', 'NO_ZERO_IN_DATE', 'NO_ZERO_DATE', 'INVALID_DATES', 'ERROR_FOR_DIVISION_BY_ZERO', 'TRADITIONAL', 'NO_AUTO_CREATE_USER', 'HIGH_NOT_PRECEDENCE', 'NO_ENGINE_SUBSTITUTION', 'PAD_CHAR_TO_FULL_LENGTH', 'EMPTY_STRING_IS_NULL', 'SIMULTANEOUS_ASSIGNMENT')</code></td><td>NO</td><td></td><td></td><td>The <a href="/kb/en/sql-mode/">SQL_MODE</a> at the time the routine was created.</td></tr>
<tr><td><code>comment</code></td><td><code>text</code></td><td>NO</td><td></td><td><code>NULL</code></td><td>Comment associated with the routine.</td></tr>
<tr><td><code>character_set_client</code></td><td><code>char(32)</code></td><td>YES</td><td></td><td><code>NULL</code></td><td>The <a href="/kb/en/data-types-character-sets-and-collations/">character set</a> used by the client that created the routine.</td></tr>
<tr><td><code>collation_connection</code></td><td><code>char(32)</code></td><td>YES</td><td></td><td><code>NULL</code></td><td>The <a href="/kb/en/data-types-character-sets-and-collations/">collation</a> (and character set) used by the connection that created the routine.</td></tr>
<tr><td><code>db_collation</code></td><td><code>char(32)</code></td><td>YES</td><td></td><td><code>NULL</code></td><td>The default <a href="/kb/en/data-types-character-sets-and-collations/">collation</a> (and character set) for the database, at the time the routine was created.</td></tr>
<tr><td><code>body_utf8</code></td><td><code>longblob</code></td><td>YES</td><td></td><td><code>NULL</code></td><td>Definition of the routine in utf8.</td></tr>
<tr><td><code>aggregate</code></td><td><code>enum('NONE', 'GROUP')</code></td><td>NO</td><td></td><td><code>NONE</code></td><td>From <a href="/kb/en/mariadb-1033-release-notes/">MariaDB 10.3.3</a></td></tr>
<tr><th>Field</th><th>Type</th><th>Null</th><th>Key</th><th>Default</th><th>Description</th></tr>
</tbody></table>

## See Also

- [Stored Procedure Internals](/programming-customizing-mariadb/stored-routines/stored-procedures/stored-procedure-internals/)
- [MySQL to MariaDB migration: handling privilege table differences when using mysqldump](https://mariadb.com/blog/mysql-mariadb-migration-handling-privilege-table-differences-when-using-mysqldump)