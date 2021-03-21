# mysql_install_db.exe

The `mysql_install_db.exe` utility is the Windows equivalent of [mysql_install_db](/clients-utilities/mysql_install_db).

## Functionality

The functionality of `mysql_install_db.exe` is comparable with the shell
script `mysql_install_db` used on Unix, however it has been extended with
both Windows specific functionality (creating a Windows service) and to
generally useful functionality. For example, it can set the 'root' user
password during database creation. It also creates the `my.ini` configuration
file in the data directory and adds most important parameters to it (e.g port).

`mysql_install_db.exe` is used by the MariaDB installer for Windows if the
"Database instance" feature is selected. It obsoletes similar utilities and
scripts that were used in the past such as `mysqld.exe` `<code>--`install</code>,
`mysql_install_db.pl`, and `mysql_secure_installation.pl`.

<table><tbody><tr><th>Parameter</th><th>Description</th></tr>
<tr><td><code>-?</code>, <code><code>--</code>help</code></td><td>Display help message and exit</td></tr>
<tr><td><code>-d</code>, <code><code>--</code>datadir=name</code></td><td>Data directory of the new database</td></tr>
<tr><td><code>-S</code>, <code><code>--</code>service=name</code></td><td>Name of the Windows service</td></tr>
<tr><td><code>-p</code>, <code><code>--</code>password=name</code></td><td>Password of the root user</td></tr>
<tr><td><code>-P</code>, <code><code>--</code>port=# </code></td><td><code>mysqld</code> port</td></tr>
<tr><td><code>-W</code>, <code><code>--</code>socket=name</code></td><td>named pipe name</td></tr>
<tr><td><code>-D</code>, <code><code>--</code>default-user</code></td><td>Create default user</td></tr>
<tr><td><code>-R</code>, <code><code>--</code>allow-remote-root-access</code></td><td>Allow remote access from network for user root</td></tr>
<tr><td><code>-N</code>, <code><code>--</code>skip-networking</code></td><td>Do not use TCP connections, use pipe instead</td></tr>
<tr><td><code>-i</code>, <code><code>--</code>innodb-page-size</code></td><td>Innodb page size, since <a href="/kb/en/mariadb-1025-release-notes/">MariaDB 10.2.5</a></td></tr>
</tbody></table>

<strong>Note </strong>: to create a Windows service, `mysql_install_db.exe` should be run
by a user with full administrator privileges (which means  elevated command
prompt on systems with UAC). For example, if you are running it on Windows 7, make sure that your command prompt was launched via 'Run as Administrator' option.

## Example

```sql
  mysql_install_db.exe --datadir=C:\db --service=MyDB --password=secret
```

will create the database in the directory C:\db, register the auto-start
Windows service "MyDB", and set the root password to 'secret'.

To start the service from the command line, execute

```sql
sc start MyDB
```

## Removing Database Instances

If you run your database instance as service, to remove it completely from the
command line, use

```sql
sc stop <servicename>
sc delete <servicename>
rmdir /s /q <path-to-datadir>
```