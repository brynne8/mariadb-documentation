# mysqlaccess

##### MariaDB starting with [10.4.6](/kb/en/mariadb-1046-release-notes/)

From [MariaDB 10.4.6](/kb/en/mariadb-1046-release-notes/),  `mariadb-access` is a symlink to `mysqlaccess`.

##### MariaDB starting with [10.5.2](/kb/en/mariadb-1052-release-notes/)

From [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), `mariadb-access` is the name of the tool, with `mysqlaccess` a symlink .

`mysqlaccess` is a tool for checking access privileges, developed by Yves Carlier.

It checks the access privileges for a host name, user name, and database combination. Note that mysqlaccess checks access using only the [user](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqluser-table/), [db](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqldb-table/), and host tables. It does not check table, column, or routine privileges specified in the [tables_priv](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqltables_priv-table/), [columns_priv](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlcolumns_priv-table/), or [procs_priv](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlprocs_priv-table/) tables.

## Usage

```sql
mysqlaccess [host [user [db]]] OPTIONS
```

If your MariaDB distribution is installed in some non-standard location, you must change the location where <em>mysqlaccess</em> expects to find the mysql client. Edit the <em>mysqlaccess</em> script at approximately line 18. Search for a line that looks like this:
&lt;&lt;code&gt;
$MYSQL     = ´/usr/local/bin/mysql´;    # path to mysql executable
&lt;&lt;/code&gt;&gt;
Change the path to reflect the location where <em>mysql</em> actually is stored on your system. If you do not do this, a <em>Broken pipe error</em> will occur when you run <em>mysqlaccess</em>.

## Options

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>-?</code>, <code>--help</code></td><td>Displayhelp and exit.</td></tr>
<tr><td><code>-v</code>, <code>--version</code></td><td>Display version.</td></tr>
<tr><td><code>-u username</code>, <code>--user=username</code></td><td>Username for logging in to the db.</td></tr>
<tr><td><code>-p[password]</code>, <code>--password[=password]</code></td><td>Password to use for user. If ommitted, <code>mysqlaccess</code> prompts for one.</td></tr>
<tr><td><code>-h hostname</code>, <code>--host=hostname </code></td><td>Name or IP of the host.</td></tr>
<tr><td><code>-d dbname</code>, <code>--db=dbname</code></td><td>Name of the database.</td></tr>
<tr><td><code>-U username</code>, <code>--superuser=username</code></td><td>Connect as superuser.</td></tr>
<tr><td><code>-P password</code>, <code>--spassword=password</code></td><td>Password for superuser.</td></tr>
<tr><td><code>-H server</code>, <code>--rhost=server</code></td><td>Remote server to connect to.</td></tr>
<tr><td><code>--old_server</code></td><td>Connect to a very old MySQL servers (before version 3.21) that does not know how to handle full WHERE clauses.</td></tr>
<tr><td><code>-b</code>, <code>--brief</code></td><td>Single-line tabular report.</td></tr>
<tr><td><code>-t</code>, <code>--table</code></td><td>Report in table-format.</td></tr>
<tr><td><code>--relnotes</code></td><td>Print release-notes.</td></tr>
<tr><td><code>--plan</code></td><td>Print suggestions/ideas for future releases.</td></tr>
<tr><td><code>--howto</code></td><td>Some examples of how to run `mysqlaccess'.</td></tr>
<tr><td><code>--debug=N</code></td><td>Enter debug level N (0..3).</td></tr>
<tr><td><code>--copy</code></td><td>Reload temporary grant-tables from original ones.</td></tr>
<tr><td><code>--preview</code></td><td>Show differences in privileges after making changes in (temporary) grant-tables.</td></tr>
<tr><td><code>--commit</code></td><td>Copy grant-rules from temporary tables to grant-tables (the grant tables must be flushed after, for example with <code><a href="/kb/en/mysqladmin/">mysqladmin reload</a></code>).</td></tr>
<tr><td><code>--rollback</code></td><td>Undo the last changes to the grant-tables.</td></tr>
</tbody></table>

## Note

At least the user (`-u`) and the database (`-d`) must be given, even with wildcards. If no host is provided, `localhost' is assumed. Wildcards (*,?,%,_) are allowed for <em>host</em>, <em>user</em> and <em>db</em>, but be sure to escape them from your shell!! (ie type \* or '*')