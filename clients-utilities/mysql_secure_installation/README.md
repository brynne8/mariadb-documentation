# mysql_secure_installation

##### MariaDB starting with [10.4.6](/kb/en/mariadb-1046-release-notes/)

From [MariaDB 10.4.6](/kb/en/mariadb-1046-release-notes/), `mariadb-secure-installation` is a symlink to `mysql_secure_installation`.

##### MariaDB starting with [10.5.2](/kb/en/mariadb-1052-release-notes/)

From [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), `mysql_secure_installation` is the symlink, and `mariadb-secure-installation` the binary name.

## Description

`mysql_secure_installation` is a shell script available on Unix systems, and enables you to improve the security of your MariaDB installation in the following ways:

- You can set a password for root accounts.
- You can remove root accounts that are accessible from outside the local host.
- You can remove anonymous-user accounts.
- You can remove the test database, which by default can be accessed by anonymous users.

`mysql_secure_installation` can be invoked without arguments:

```sql
shell> mysql_secure_installation```

The script will prompt you to determine which actions to perform.

```sql
Example:
localhost:# mysql_secure_installation 
NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!
In order to log into MariaDB to secure it, we'll need the current
password for the root user.  If you've just installed MariaDB, and
you haven't set the root password yet, the password will be blank,
so you should just press enter here.
Enter current password for root (enter for none): 
OK, successfully used password, moving on...
Setting the root password ensures that nobody can log into the MariaDB
root user without the proper authorisation.
You already have a root password set, so you can safely answer 'n'.
Change the root password? [Y/n] n
 ... skipping.
By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.
Remove anonymous users? [Y/n] y
 ... Success!
Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.
Disallow root login remotely? [Y/n] y
 ... Success!
By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.
Remove test database and access to it? [Y/n] y
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!
Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.
Reload privilege tables now? [Y/n] y
 ... Success!
Cleaning up...
All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.
Thanks for using MariaDB!```

### Options

`mysql_secure_installation` accepts some options:

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>--basedir=dir</code></td><td>Base directory.</td></tr>
<tr><td><code>--print-defaults</code></td><td>Print the program argument list and exit.</td></tr>
<tr><td><code>--no-defaults</code></td><td>Don't read default options from any option file.</td></tr>
<tr><td><code>--defaults-file=# </code></td><td>Only read default options from the given file #.</td></tr>
<tr><td><code>--defaults-extra-file=# </code></td><td>Read this file after the global files are read.</td></tr>
</tbody></table>

Other unrecognized options will be passed on to the server.

### Option Files

In addition to reading options from the command-line, `mysql_secure_installation` can also read options from [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files). If an unknown option is provided to `mysql_secure_installation` in an option file, then it is ignored.

The following options relate to how MariaDB command-line tools handles option files. They must be given as the first argument on the command-line:

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>--print-defaults</code></td><td>Print the program argument list and exit.</td></tr>
<tr><td><code>--no-defaults</code></td><td>Don't read default options from any option file.</td></tr>
<tr><td><code>--defaults-file=# </code></td><td>Only read default options from the given file #.</td></tr>
<tr><td><code>--defaults-extra-file=# </code></td><td>Read this file after the global files are read.</td></tr>
<tr><td><code>--defaults-group-suffix=# </code></td><td>In addition to the default option groups, also read option groups with this suffix.</td></tr>
</tbody></table>

#### Option Groups

`mysql_secure_installation` reads options from the following [option groups](/kb/en/configuring-mariadb-with-option-files/#option-groups) from [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files):

<table><tbody><tr><th>Group</th><th>Description</th></tr>
<tr><td><code>[client]</code></td><td>&nbsp;Options read by all MariaDB and MySQL <a href="/kb/en/clients-utilities/">client programs</a>, which includes both MariaDB and MySQL clients. For example, <code>mysqldump</code>.</td></tr>
<tr><td><code>[client-server]</code></td><td>Options read by all MariaDB <a href="/kb/en/clients-utilities/">client programs</a> and the MariaDB Server. This is useful for options like socket and port, which is common between the server and the clients.</td></tr>
<tr><td><code>[client-mariadb]</code></td><td>Options read by all MariaDB <a href="/kb/en/clients-utilities/">client programs</a>.</td></tr>
</tbody></table>

### Use With Galera Cluster

This script is not 100% safe for use with [Galera Cluster](/kb/en/galera/) as it directly manipulates the [mysql.user Table](/kb/en/mysqluser-table/) which is not transported by Galera to the other nodes.

You should run this script on the first node in the cluster before adding more nodes.

If you want to run this after the cluster is up and running you should find alternative ways.

Anyone can vote for this to be fixed at [https://jira.mariadb.org/browse/MDEV-10112](https://jira.mariadb.org/browse/MDEV-10112).