# mysql_setpermission

##### MariaDB starting with [10.4.6](/kb/en/mariadb-1046-release-notes/)

From [MariaDB 10.4.6](/kb/en/mariadb-1046-release-notes/), `mariadb-setpermission` is a symlink to `mysql_setpermission`.

##### MariaDB starting with [10.5.2](/kb/en/mariadb-1052-release-notes/)

From [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), `mysql_setpermission` is the symlink, and `mariadb-setpermission` the binary name.

## Syntax

```sql
mysql_setpermission [options]
```

## Description

<em>mysql_setpermission</em> is a Perl script that was originally written and contributed by Luuk de Boer. It requires the DBI and DBD::mysql Perl modules to be installed.
<em>mysql_setpermission</em> can help you add users or databases or change passwords in MariaDB.

It interactively sets permissions in the MariaDB grant tables, but does not check permissions which have already been set in MariaDB. So if you can't connect to MariaDB using the permission you just added, take a look at the permissions which have already been set in MariaDB.

The account used when you connect determines which permissions you have when attempting to modify existing permissions in the grant tables.

<em>mysql_setpermission</em> also reads options from the [client] and [perl] groups in the .my.cnf file in your home directory, if the file exists.

The following options are available:

## Options

<table><tbody><tr><th>Option</th><th>Description</th></tr>
</tbody></table>

<table><tbody><tr><td><code>--help</code></td><td>Display a help message and exit.</td></tr>
<tr><td><code>--host=host_name</code></td><td>Connect to the MariaDB server on the given host.</td></tr>
<tr><td><code>--password=password</code></td><td>The password to use when connecting to the server. Note that the password value is not optional for this option, unlike for other MariaDB programs<em><em> Specifying a password on the command line should be considered insecure. You can use an option file to avoid giving the password on the command line.</em></em></td></tr>
<tr><td><code>--port=port_num</code></td><td>The TCP/IP port number to use for the connection.</td></tr>
<tr><td><code>--socket=path</code></td><td>For connections to localhost, the Unix socket file to use.</td></tr>
<tr><td><code>--user=user_name</code></td><td>The MariaDB user name to use when connecting to the server.</td></tr>
</tbody></table>