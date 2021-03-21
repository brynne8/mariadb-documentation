# mysql_install_db

##### MariaDB starting with [10.4.6](/kb/en/mariadb-1046-release-notes/)

From [MariaDB 10.4.6](/kb/en/mariadb-1046-release-notes/), `mariadb-install-db` is a symlink to `mysql_install_db`.

##### MariaDB starting with [10.5.2](/kb/en/mariadb-1052-release-notes/)

From [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), `mysql_install_db` is the symlink, and `mariadb-install-db` the binary name.

`mysql_install_db` initializes the MariaDB data directory and creates the
[system tables](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables) in the [mysql](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables) database, if they do not exist. MariaDB uses these tables to manage [privileges](/kb/en/grant/#privilege-levels), [roles](/mariadb-administration/user-server-security/user-account-management/roles), and [plugins](/columns-storage-engines-and-plugins/plugins). It also uses them to provide the data for the [help](/sql-statements-structure/sql-statements/administrative-sql-statements/help-command) command in the [mysql](/clients-utilities/mysql-client/mysql-command-line-client) client.

`mysql_install_db` works by starting MariaDB Server's `mysqld` process in <a undefined>--bootstrap</a> mode and sending commands to create the [system tables](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables) and their content.

There is a version specifically for Windows, [mysql_install_db.exe](/mariadb-administration/getting-installing-and-upgrading-mariadb/mysql_install_dbexe). The Windows version shares the common theme (creating system tables), yet has a lot functionality specific to Windows systems, for example creating a Windows service. The Windows version does *not* share parameters with the Unix shell script, and if you are looking for Windows specific instruction, refer to [mysql_install_db.exe](/mariadb-administration/getting-installing-and-upgrading-mariadb/mysql_install_dbexe) instead of reading this page.

## Using mysql_install_db

To invoke `mysql_install_db`, use the following syntax:

```sql
$ mysql_install_db [options]
```

Because the MariaDB server, `mysqld`, needs to access the data directory
when it runs later, you should either run `mysql_install_db` from the same
account that will be used for running `mysqld` or run it as root and use the
`--user` option to indicate the user name that `mysqld` will run
as. It might be necessary to specify other options such as
`--basedir` or `--datadir` if
`mysql_install_db` does not use the correct locations for the installation
directory or data directory. For example:

```sql
$ scripts/mysql_install_db --user=mysql \
   --basedir=/opt/mysql/mysql \
   --datadir=/opt/mysql/mysql/data
```

### Options

`mysql_install_db` supports the following options:

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>--auth-root-authentication-method={normal <code>|</code> socket}</code></td><td>If set to <code>normal</code>, it creates a <code>root@localhost</code> account that authenticates with the <code><a href="/kb/en/authentication-plugin-mysql_native_password/">mysql_native_password</a></code> authentication plugin and that has no initial password set, which can be insecure. If set to <code>socket</code>, it creates a <code>root@localhost</code> account that authenticates with the <code><a href="/kb/en/authentication-plugin-unix-socket/">unix_socket</a></code> authentication plugin. Set to <code>socket</code> by default from <a href="/kb/en/what-is-mariadb-104/">MariaDB 10.4</a> (see <a href="/kb/en/authentication-from-mariadb-104/">Authentication from MariaDB 10.4</a>), or <code>normal</code> by default in earlier versions. Available since <a href="/kb/en/what-is-mariadb-101/">MariaDB 10.1</a>.</td></tr>
<tr><td><code>--auth-root-socket-user=USER</code></td><td>Used with <code>--auth-root-authentication-method=socket</code>. It specifies the name of the second account to create with <code><a href="/kb/en/grant/#global-privileges">SUPER</a></code> privileges in addition to <code>root</code>, as well as of the system account allowed to access it. Defaults to the value of <code>--user</code>.</td></tr>
<tr><td><code>--basedir=path</code></td><td>The path to the MariaDB installation directory.</td></tr>
<tr><td><code>--builddir=path</code></td><td>If using <code>--srcdir</code> with out-of-directory builds, you will need to set this to the location of the build directory where built files reside.</td></tr>
<tr><td><code>--cross-bootstrap</code></td><td>For internal use.  Used when building the MariaDB system tables on a different host than the target.</td></tr>
<tr><td><code>--datadir=path</code>, <code>--ldata=path</code></td><td>The path to the MariaDB data directory.</td></tr>
<tr><td><code>--defaults-extra-file=name</code></td><td>Read this file after the global files are read. Must be given as the first option.</td></tr>
<tr><td><code>--defaults-file=name</code></td><td>Only read default options from the given file <em>name</em> Must be given as the first option.</td></tr>
<tr><td><code>--defaults-group-suffix=name</code></td><td>In addition to the given groups, read also groups with this suffix. From <a href="/kb/en/mariadb-10131-release-notes/">MariaDB 10.1.31</a>, <a href="/kb/en/mariadb-10213-release-notes/">MariaDB 10.2.13</a> and <a href="/kb/en/mariadb-1035-release-notes/">MariaDB 10.3.5</a>.</td></tr>
<tr><td><code>--force</code></td><td>Causes <code>mysql_install_db</code> to run even if DNS does not work. In that case, grant table entries that normally use host names will use IP addresses.</td></tr>
<tr><td><code>--no-defaults</code></td><td>Don't read default options from any option file. Must be given as the first option.</td></tr>
<tr><td><code>--print-defaults</code></td><td>Print the program argument list and exit. Must be given as the first option.</td></tr>
<tr><td><code>--rpm</code></td><td>For internal use. This option is used by RPM files during the MariaDB installation process.</td></tr>
<tr><td><code>--skip-auth-anonymous-user</code></td><td>Do not create the anonymous user.</td></tr>
<tr><td><code>--skip-name-resolve</code></td><td>Uses IP addresses rather than host names when creating grant table entries. This option can be useful if your DNS does not work.</td></tr>
<tr><td><code>--skip-test-db</code></td><td>Don't install the test database.</td></tr>
<tr><td><code>--srcdir=path</code></td><td>For internal use. The path to the MariaDB source directory. This option uses the compiled binaries and support files within the source tree, useful for if you don't want to install MariaDB yet and just want to create the system tables. The directory under which <code>mysql_install_db</code> looks for support files such as the error message file and the file for populating the help tables.</td></tr>
<tr><td><code>--user=user_name</code></td><td>The login user name to use for running <code>mysqld</code>. Files and directories created by <code>mysqld</code> will be owned by this user. You must be <code>root</code> to use this option. By default, <code>mysqld</code> runs using your current login name and files and directories that it creates will be owned by you.</td></tr>
<tr><td><code>--verbose</code></td><td>Verbose mode. Print more information about what the program does.</td></tr>
<tr><td><code>--windows</code></td><td>For internal use. This option is used for creating Windows distributions.</td></tr>
</tbody></table>

### Option Files

In addition to reading options from the command-line, `mysql_install_db` can also read options from [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files). If an unknown option is provided to `mysql_install_db` in an option file, then it is ignored.

The following options relate to how MariaDB command-line tools handles option files. They must be given as the first argument on the command-line:

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>--print-defaults</code></td><td>Print the program argument list and exit.</td></tr>
<tr><td><code>--no-defaults</code></td><td>Don't read default options from any option file.</td></tr>
<tr><td><code>--defaults-file=# </code></td><td>Only read default options from the given file #.</td></tr>
<tr><td><code>--defaults-extra-file=# </code></td><td>Read this file after the global files are read.</td></tr>
<tr><td><code>--defaults-group-suffix=# </code></td><td>In addition to the default option groups, also read option groups with this suffix.</td></tr>
</tbody></table>

#### Option Groups

`mysql_install_db` reads options from the following [option groups](/kb/en/configuring-mariadb-with-option-files/#option-groups) from [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files):

<table><tbody><tr><th>Group</th><th>Description</th></tr>
<tr><td><code>[mysql_install_db]</code></td><td>&nbsp;Options read by <code>mysqld_safe</code>, which includes both MariaDB Server and MySQL Server.</td></tr>
</tbody></table>

`mysql_install_db` also reads options from the following server [option groups](/kb/en/configuring-mariadb-with-option-files/#option-groups) from [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files):

<table><tbody><tr><th>Group</th><th>Description</th></tr>
<tr><td><code>[mysqld]</code></td><td>&nbsp;Options read by <code>mysqld</code>, which includes both MariaDB Server and MySQL Server.</td></tr>
<tr><td><code>[server]</code></td><td>Options read by MariaDB Server.</td></tr>
<tr><td><code>[mysqld-X.Y]</code></td><td>&nbsp;Options read by a specific version of <code>mysqld</code>, which includes both MariaDB Server and MySQL Server. For example, <code>[mysqld-5.5]</code>.</td></tr>
<tr><td><code>[mariadb]</code></td><td>Options read by MariaDB Server.</td></tr>
<tr><td><code>[mariadb-X.Y]</code></td><td>&nbsp;Options read by a specific version of MariaDB Server.</td></tr>
<tr><td><code>[client-server]</code></td><td>Options read by all MariaDB <a href="/kb/en/clients-utilities/">client programs</a> and the MariaDB Server. This is useful for options like socket and port, which is common between the server and the clients.</td></tr>
<tr><td><code>[galera]</code></td><td>&nbsp;Options read by a galera-capable MariaDB Server. Available on systems compiled with Galera support.</td></tr>
</tbody></table>

## Installing System Tables

### Installing System Tables From a Source Tree

If you have just [compiled MariaDB from source](/mariadb-administration/getting-installing-and-upgrading-mariadb/compiling-mariadb-from-source), and if you want to use `mysql_install_db` from your source tree, then that can be done without having to actually install MariaDB. This is very useful if you want to test your changes to MariaDB without disturbing any existing installations of MariaDB.

To do so, you would have to provide the `--srcdir` option. For example:

```sql
./scripts/mysql_install_db --srcdir=. --datadir=path-to-temporary-data-dir
```

### Installing System Tables From a Binary Tarball

If you install a [binary tarball](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-binary-tarballs) package in a non standard path, like your home directory, and if you already have a MariaDB / MySQL package installed, then you may get conflicts
with the default `/etc/my.cnf`. This often results in permissions
errors.

One possible solution is to use the `--no-defaults` option, so that it does not read any [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files). For example:

```sql
./scripts/mysql_install_db --no-defaults --basedir=. --datadir=data
```

Another possible solution is to use the `defaults-file` option, so that you can specify your own [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files). For example:

```sql
./scripts/mysql_install_db --defaults-file=~/.my.cnf
```

## User Accounts Created by Default

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, `mysql_install_db` sets `--auth-root-authentication-method=socket` by default. When this is set, the default `root@localhost` user account is created with the ability to use two [authentication plugins](/columns-storage-engines-and-plugins/plugins/authentication-plugins):

- First, it is configured to try to use the [unix_socket](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-unix-socket) authentication plugin. This allows the the `root@localhost` user to login without a password via the local Unix socket file defined by the <a undefined>socket</a> system variable, as long as the login is attempted from a process owned by the operating system `root` user account.
- Second, if authentication fails with the [unix_socket](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-unix-socket) authentication plugin, then it is configured to try to use the [mysql_native_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_native_password) authentication plugin.

The definition of the default `root@localhost` user account is:

```sql
CREATE USER 'root'@'localhost' IDENTIFIED VIA unix_socket OR mysql_native_password USING 'invalid';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' WITH GRANT OPTION;
GRANT PROXY ON ''@'%' TO 'root'@'localhost' WITH GRANT OPTION;
```

Since `mysql_install_db` sets `--auth-root-authentication-method=socket` by default, the following additional user accounts are <strong>not</strong> created by default:

- `root@127.0.0.1`
- `root@::1`
- `root@${current_hostname}`

However, an additional user account that is defined by the `--auth-root-socket-user` option is created. If this option is not set, then the value defaults to the value of the `--user` option. On most systems, the `--user` option will use the value of `mysql` by default, so this additional user account would be called `mysql@localhost`.

The definition of this `mysql@localhost` user account is similar to the `root@localhost` user account:

```sql
CREATE USER 'mysql'@'localhost' IDENTIFIED VIA unix_socket OR mysql_native_password USING 'invalid';
GRANT ALL PRIVILEGES ON *.* TO 'mysql'@'localhost' WITH GRANT OPTION;
```

An invalid password is initially set for both of these user accounts. This means that before a password can be used to authenticate as either of these user accounts, the accounts must first be given a valid password by executing the [SET PASSWORD](/sql-statements-structure/sql-statements/account-management-sql-commands/set-password) statement.

For example, here is an example of setting the password for the `root@localhost` user account immediately after installation:

```sql
$ sudo yum install MariaDB-server
$ sudo systemctl start mariadb
$ sudo mysql
...
MariaDB> SET PASSWORD = PASSWORD('XH4VmT3_jt');
```

You may notice in the above example that the [mysql](/clients-utilities/mysql-client/mysql-command-line-client) command-line client is executed via <a undefined>sudo</a>. This allows the `root@localhost` user account to successfully authenticate via the [unix_socket](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-unix-socket) authentication plugin.

##### MariaDB until [10.3](/kb/en/what-is-mariadb-103/)

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and before, `mysql_install_db` sets `--auth-root-authentication-method=normal` by default. When this is set, the following default accounts are created with no password:

- `root@localhost`
- `root@127.0.0.1`
- `root@::1`
- `root@${current_hostname}`

The definition of the default `root@localhost` user account is:

```sql
CREATE USER 'root'@'localhost';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' WITH GRANT OPTION;
GRANT PROXY ON ''@'%' TO 'root'@'localhost' WITH GRANT OPTION;
```

The definition of the other default `root` accounts is similar.

A password should be set for these user accounts immediately after installation. This can be done either by executing the [SET PASSWORD](/sql-statements-structure/sql-statements/account-management-sql-commands/set-password) statement or by running [mysql_secure_installation](/clients-utilities/mysql_secure_installation).

For example, here is an example of setting the password for the `root@localhost` user account immediately after installation:

```sql
$ sudo yum install MariaDB-server
$ sudo systemctl start mariadb
$ mysql -u root
...
MariaDB> SET PASSWORD = PASSWORD('XH4VmT3_jt');
```

Since `mysql_install_db` sets `--auth-root-authentication-method=normal` by default, the `--auth-root-socket-user` option is ignored by default.

## Troubleshooting Issues

### Checking the Error Log

If `mysql_install_db` fails, you should examine the [error log](/mariadb-administration/server-monitoring-logs/error-log) in the
data directory, which is the directory specified with `--datadir` option. This should provide a clue about what went wrong.

### Testing With mysqld

You can also test that this is not a general fault of MariaDB Server by trying to start the `mysqld` process. The <a undefined>-skip-grant-tables</a> option will tell it to ignore the [system tables](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables). Enabling the [general query log](/mariadb-administration/server-monitoring-logs/general-query-log) can help you determine what queries are being run on the server. For example:

```sql
mysqld --skip-grant-tables --general-log
```

At this point, you can use the [mysql](/clients-utilities/mysql-client/mysql-command-line-client) client to connect to the  [mysql](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables) database and look at the [system tables](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables). For example:

```sql
$ /usr/local/mysql/bin/mysql -u root mysql
MariaDB [mysql]> show tables
```

## Using a Server Compiled With --disable-grant-options

The following only apply in the exceptional case that you are using a mysqld server which is configured with the <code class="fixed" style="white-space:pre-wrap">--disable-grant-options</code> option:

`mysql_install_db` needs to invoke `mysqld` with the
<code class="fixed" style="white-space:pre-wrap">--bootstrap</code> and <code class="fixed" style="white-space:pre-wrap">--skip-grant-tables</code> options.
A MariaDB configured with the <code class="fixed" style="white-space:pre-wrap">--disable-grant-options</code>
option has <code class="fixed" style="white-space:pre-wrap">--bootstrap</code> and <code class="fixed" style="white-space:pre-wrap">--skip-grant-tables</code>
disabled. To handle this case, set the `MYSQLD_BOOTSTRAP` environment
variable to the full path name of a mysqld server that is configured without <code class="fixed" style="white-space:pre-wrap">--disable-grant-options</code>. `mysql_install_db` will use that server.

## The test and test_% Databases

When calling the `mysql_install_db` script, a new folder called `test` is created in the data directory.
It only has the single `db.opt` file, which sets the client options `default-character-set` and `default-collation` only.

If you run `mysql` as an anonymous user, <code class="fixed" style="white-space:pre-wrap"> mysql -u''@localhost</code>, and look for the grants and databases you are able to work with, you will get the following:

```sql
SELECT current_user;
+--------------+
| current_user |
+--------------+
| @localhost   |
+--------------+

SHOW GRANTS FOR current_user;
+--------------------------------------+
| Grants for @localhost                |
+--------------------------------------+
| GRANT USAGE ON *.* TO ``@`localhost` |
+--------------------------------------+

SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| test               |
+--------------------+
```

Shown are the `information_schema` as well as `test` databases that are built in databases.
But looking from [SHOW GRANTS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-grants) appears to be a paradox; how can the current user see something if they don't have privileges for that?

Let's go a step further.<br>
Now, use the `root`/`unix` user, which has all rights, in order to create a new database with the prefix `test_` , something like:

```sql
CREATE DATABASE test_electricity;
```

With the above change, a new directory will be created in the data directory.<br>
Now login again with the anonymous user and run [SHOW DATABASES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-databases):

```sql
SHOW DATABASES
+--------------------+
| Database           |
+--------------------+
| information_schema |
| test               |
| test_electricity   |
+--------------------+
```

Again we are able to see the newly created database, without any rights?
We have an anonymous user that has no privileges, but still can see the `test` and `test_electricity` databases.<br>
<strong>Where does this come from?</strong>

---

Login with the `root`/`unix` user to find out all privileges that the anonymous user has:

```sql
SELECT * FROM mysql.user WHERE user='' AND host='localhost'\G
*************************** 1. row ***************************
                  Host: localhost
                  User: 
              Password: 
           Select_priv: N
           Insert_priv: N
           Update_priv: N
           Delete_priv: N
           Create_priv: N
             Drop_priv: N
           Reload_priv: N
         Shutdown_priv: N
          Process_priv: N
             File_priv: N
            Grant_priv: N
       References_priv: N
            Index_priv: N
            Alter_priv: N
          Show_db_priv: N
            Super_priv: N
 Create_tmp_table_priv: N
      Lock_tables_priv: N
          Execute_priv: N
       Repl_slave_priv: N
      Repl_client_priv: N
      Create_view_priv: N
        Show_view_priv: N
   Create_routine_priv: N
    Alter_routine_priv: N
      Create_user_priv: N
            Event_priv: N
          Trigger_priv: N
Create_tablespace_priv: N
   Delete_history_priv: N
              ssl_type: 
            ssl_cipher: 
           x509_issuer: 
          x509_subject: 
         max_questions: 0
           max_updates: 0
       max_connections: 0
  max_user_connections: 0
                plugin: 
 authentication_string: 
      password_expired: N
               is_role: N
          default_role: 
    max_statement_time: 0.000000
```

As seen above from the [mysql.user](/kb/en/mysqluser-table/) table, the anonymous user doesn't have any global privileges.
Still, the anonymous user can see databases, so there must be a way so that anonymous user can see the `test` and `test_electricity` databases.

Let's check for grants on the database level. That information can be found in the [mysql.db](/kb/en/mysqldb-table/) table.
Looking at the `mysql.db` table, it already contains 2 rows created when the `mysql_install_db` script was invoked.

The anonymous user has database privileges (without `grant`, `alter_routine` and `execute`) on `test` and `test_%` databases:

```sql
SELECT * FROM mysql.db\G
*************************** 1. row ***************************
                 Host: %
                   Db: test
                 User: 
          Select_priv: Y
          Insert_priv: Y
          Update_priv: Y
          Delete_priv: Y
          Create_priv: Y
            Drop_priv: Y
           Grant_priv: N
      References_priv: Y
           Index_priv: Y
           Alter_priv: Y
Create_tmp_table_priv: Y
     Lock_tables_priv: Y
     Create_view_priv: Y
       Show_view_priv: Y
  Create_routine_priv: Y
   Alter_routine_priv: N
         Execute_priv: N
           Event_priv: Y
         Trigger_priv: Y
  Delete_history_priv: Y
*************************** 2. row ***************************
                 Host: %
                   Db: test\_%
                 User: 
          Select_priv: Y
          Insert_priv: Y
          Update_priv: Y
          Delete_priv: Y
          Create_priv: Y
            Drop_priv: Y
           Grant_priv: N
      References_priv: Y
           Index_priv: Y
           Alter_priv: Y
Create_tmp_table_priv: Y
     Lock_tables_priv: Y
     Create_view_priv: Y
       Show_view_priv: Y
  Create_routine_priv: Y
   Alter_routine_priv: N
         Execute_priv: N
           Event_priv: Y
         Trigger_priv: Y
  Delete_history_priv: Y
```

The first row is reserved for explicit usage for the `test` database, which is automatically created with `mysql_install_db`.

Since database `test_electricity` satisfies the `test_%` pattern where `test_` is a prefix, we can understand why the user has the right to work with the newly-created database.

As long as records in `mysql.db` for the anonymous user exists, each new user created will have the privileges for the `test` and `test_%` databases.

Other databases privileges <strong>are not automatically granted</strong> for the newly created user. We have to grant privileges, which will be visible in `mysql.db` table.

### Not Creating the test Database and Anonymous User

If you run `mysql_install_db` with the `--skip-test-db` option, no `test` database will be created, which we can see as follows:

```sql
SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
+--------------------+

SELECT * FROM mysql.db;
Empty set (0.001 sec)
```

Also, no anonymous user is created (only `unix`/`mariadb.sys`/`root` users):

```sql
SELECT user,host FROM mysql.user;
+-------------+-----------+
| User        | Host      |
+-------------+-----------+
| anel        | localhost |
| mariadb.sys | localhost |
| root        | localhost |
+-------------+-----------+
```

## See Also

- [Installing system tables (mysql_install_db)](/mariadb-administration/getting-installing-and-upgrading-mariadb/installing-system-tables-mysql_install_db)
- The Windows version of `mysql_install_db`: [mysql_install_db.exe](/mariadb-administration/getting-installing-and-upgrading-mariadb/mysql_install_dbexe)