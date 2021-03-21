# User and Group Mapping with PAM

Even when using the [pam](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-with-pluggable-authentication-modules-pam/authentication-plugin-pam) authentication plugin, the authenticating PAM user account still needs to exist in MariaDB, and the account needs to have privileges in the database. Creating these MariaDB accounts and making sure the privileges are correct can be a lot of work. To decrease the amount of work involved, some users would like to be able to map a PAM user to a different MariaDB user. For example, let’s say that `alice` and `bob` are both DBAs. It would be nice if each of them could log into MariaDB with their own PAM username and password, while MariaDB sees both of them as the same `dba` user. That way, there is only one MariaDB account to keep track of.

Although most PAM modules usually do not do things like this, PAM supports the ability to change the user name in the process of authentication.The MariaDB `pam` authentication plugin fully supports this feature of PAM.

## The pam_user_map PAM Module

Rather than building user and group mapping into the `pam` authentication plugin, MariaDB thought that it would cover the most use cases and offer the most flexibility to offload this functionality to an external PAM module. The `pam_user_map` PAM module was implemented by MariaDB to facilitate this. This PAM module can be [configured in the PAM service](/kb/en/authentication-plugin-pam/#configuring-the-pam-service) used by the `pam` authentication plugin, just like other PAM modules.

### Lack of Support for MySQL/Percona Group Mapping Syntax

Unlike MariaDB, MySQL and Percona implemented group mapping in their PAM authentication plugins. If you've read through [MySQL's PAM authentication documentation on group mapping](https://dev.mysql.com/doc/refman/8.0/en/pam-pluggable-authentication.html#pam-authentication-unix-with-proxy) or [Percona's PAM authentication documentation on group mapping](https://www.percona.com/doc/percona-server/8.0/management/pam_plugin.html#supplementary-groups-support), you've probably seen syntax where the group mappings are provided in the [CREATE USER](/sql-statements-structure/sql-statements/account-management-sql-commands/create-user) statement like this:

```sql
CREATE USER ''@''
  IDENTIFIED WITH authentication_pam
  AS 'mysql, root=developer, users=data_entry';
```

Since MariaDB's user and group mapping is performed by an external PAM module, MariaDB's `pam` authentication plugin does not support this syntax. Instead, the user and group mappings for the `pam_user_map` PAM module are configured in an external configuration file. This is discussed in a later section.

## Installing the pam_user_map PAM Module

The `pam_user_map` PAM module is not currently packaged or built by MariaDB, so installing it from source is currently the only option. See [MDEV-17292](https://jira.mariadb.org/browse/MDEV-17292) about that.

### Installing the pam_user_map PAM Module from Source

#### Installing Compilation Dependencies

Before the module can be compiled from source, you may need to install some dependencies.

On RHEL, CentOS, and other similar Linux distributions that use [RPM packages](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm), you need to install `gcc` and `pam-devel`. For example:

```sql
sudo yum install gcc pam-devel
```

On Debian, Ubuntu, and other similar Linux distributions that use [DEB packages](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-deb-files), you need to install `gcc` and `libpam0g-dev`. For example:

```sql
sudo apt-get install gcc libpam0g-dev
```

#### Compiling and Installing the pam_user_map PAM Module

The `pam_user_map` PAM module can easily be built by downloading `plugin/auth_pam/mapper/pam_user_map.c` file from the MariaDB source tree and compiling it. Once it is built, it can be installed to the system's PAM module directory, which is typically `/lib64/security/`. For example:

```sql
wget https://raw.githubusercontent.com/MariaDB/server/10.4/plugin/auth_pam/mapper/pam_user_map.c
gcc pam_user_map.c -shared -lpam -fPIC -o pam_user_map.so
sudo install --mode=0755 pam_user_map.so /lib64/security/
```

## Configuring the pam_user_map PAM Module

The `pam_user_map` PAM module uses the configuration file at the path `/etc/security/user_map.conf` to determine its user and group mappings. The file's format is described below.

To map a specific PAM user to a specific MariaDB user:

```sql
orig_pam_user_name: mapped_mariadb_user_name
```

Or to map any PAM user in a specific PAM group to a specific MariaDB user, the group name is prefixed with `@`:

```sql
@orig_pam_group_name: mapped_mariadb_user_name
```

For example, here is an example `/etc/security/user_map.conf`:

```sql
=========================================================
#comments and empty lines are ignored
john: jack
bob:  admin
top:  accounting
@group_ro: readonly
```

## Configuring PAM

With user and group mapping, configuring PAM is done similar to how it is [normally done with the `pam` authentication plugin](/kb/en/authentication-plugin-pam/#configuring-pam). However, when configuring the PAM service, you will have to add an `auth` line for the `pam_user_map` PAM module to the service's PAM configuration file. For example:

```sql
auth required pam_unix.so audit
auth required pam_user_map.so
account required pam_unix.so audit
```

## Creating Users

With user and group mapping, creating users is done similar to how it is [normally done with the `pam` authentication plugin](/kb/en/authentication-plugin-pam/#creating-users). However, one major difference is that you will need to [GRANT](/sql-statements-structure/sql-statements/account-management-sql-commands/grant) the <a undefined>PROXY</a> privilege on the mapped user to the original user.

For example, if you have the following configured in `/etc/security/user_map.conf`:

```sql
foo: bar
@dba:dba
```

Then you could execute the following to grant the relevant privileges:

```sql
CREATE USER 'bar'@'%' IDENTIFIED BY 'strongpassword';
GRANT ALL PRIVILEGES ON *.* TO 'bar'@'%' ;

CREATE USER 'dba'@'%' IDENTIFIED BY 'strongpassword';
GRANT ALL PRIVILEGES ON *.* TO 'dba'@'%' ;

CREATE USER ''@'%' IDENTIFIED VIA pam USING 'mariadb';
GRANT PROXY ON 'bar'@'%' TO ''@'%';
GRANT PROXY ON 'dba'@'%' TO ''@'%';
```

Note that the `''@'%'` account is a special catch-all [anonymous account](/kb/en/create-user/#anonymous-accounts). Any login by a user that has no more specific account match in the system will be matched by this anonymous account.

Also note that you might not be able to create the `''@'%'` anonymous account by default on some systems without doing some extra steps first. See [Fixing a Legacy Default Anonymous Account](/kb/en/create-user/#fixing-a-legacy-default-anonymous-account) for more information.

## Verifying that Mapping is Occurring

In case any user mapping is performed, the original user name is returned by the SQL function [USER()](/built-in-functions/secondary-functions/information-functions/user), while the authenticated user name is returned by the SQL function [CURRENT_USER()](/built-in-functions/secondary-functions/information-functions/current_user). The latter actually defines what privileges are available to a connected user.

For example, if we have the following configured:

```sql
foo: bar
```

Then the following output would verify that it is working properly:

```sql
$ mysql -u foo -h 172.30.0.198
[mariadb] Password:
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 22
Server version: 10.3.10-MariaDB MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> SELECT USER(), CURRENT_USER();
+------------------------------------------------+----------------+
| USER()                                         | CURRENT_USER() |
+------------------------------------------------+----------------+
| foo@ip-172-30-0-198.us-west-2.compute.internal | bar@%          |
+------------------------------------------------+----------------+
1 row in set (0.000 sec)
```

We can verify that our `foo` PAM user was properly mapped to the `bar` MariaDB user by looking at the return value of [CURRENT_USER()](/built-in-functions/secondary-functions/information-functions/current_user).

## Logging

By default, the `pam_user_map` PAM module does not perform any logging. However, if you want to enable debug logging, then you can add the `debug` module argument to the service's PAM configuration file. For example:

```sql
auth required pam_unix.so audit
auth required pam_user_map.so debug
account required pam_unix.so audit
```

When debug logging is enabled, the `pam_user_map` PAM module will write log entries to the [same syslog location](/kb/en/authentication-plugin-pam/#logging) as other PAM modules, which is typically `/var/log/secure` on many systems.

For example, this debug log output can look like the following:

```sql
Jan  9 05:42:13 ip-172-30-0-198 mysqld: pam_user_map(mariadb:auth): Opening file '/etc/security/user_map.conf'.
Jan  9 05:42:13 ip-172-30-0-198 mysqld: pam_user_map(mariadb:auth): Incoming username 'alice'.
Jan  9 05:42:13 ip-172-30-0-198 mysqld: pam_user_map(mariadb:auth): User belongs to 2 groups [alice,dba].
Jan  9 05:42:13 ip-172-30-0-198 mysqld: pam_user_map(mariadb:auth): Check if user is in group 'dba': YES
Jan  9 05:42:13 ip-172-30-0-198 mysqld: pam_user_map(mariadb:auth): User mapped as 'dba'
Jan  9 05:43:36 ip-172-30-0-198 mysqld: pam_user_map(mariadb:auth): Opening file '/etc/security/user_map.conf'.
Jan  9 05:43:36 ip-172-30-0-198 mysqld: pam_user_map(mariadb:auth): Incoming username 'bob'.
Jan  9 05:43:36 ip-172-30-0-198 mysqld: pam_user_map(mariadb:auth): User belongs to 2 groups [bob,dba].
Jan  9 05:43:36 ip-172-30-0-198 mysqld: pam_user_map(mariadb:auth): Check if user is in group 'dba': YES
Jan  9 05:43:36 ip-172-30-0-198 mysqld: pam_user_map(mariadb:auth): User mapped as 'dba'
Jan  9 06:08:36 ip-172-30-0-198 mysqld: pam_user_map(mariadb:auth): Opening file '/etc/security/user_map.conf'.
Jan  9 06:08:36 ip-172-30-0-198 mysqld: pam_user_map(mariadb:auth): Incoming username 'foo'.
Jan  9 06:08:36 ip-172-30-0-198 mysqld: pam_user_map(mariadb:auth): User belongs to 1 group [foo].
Jan  9 06:08:36 ip-172-30-0-198 mysqld: pam_user_map(mariadb:auth): Check if user is in group 'dba': NO
Jan  9 06:08:36 ip-172-30-0-198 mysqld: pam_user_map(mariadb:auth): Check if username 'foo': YES
Jan  9 06:08:36 ip-172-30-0-198 mysqld: pam_user_map(mariadb:auth): User mapped as 'bar'
```

## Known Issues

### PAM User with Same Name as Mapped MariaDB User Must Exist

With user and group mapping, any PAM user or any PAM user in a given PAM group can be mapped to a specific MariaDB user account. However, due to the way PAM works, a PAM user with the same name as the mapped MariaDB user account must exist.

For example, if the configuration file for the PAM service file contained the following:

```sql
auth required pam_sss.so
auth required pam_user_map.so debug
account sufficient pam_unix.so
account sufficient pam_sss.so
```

And if `/etc/security/user_map.conf` contained the following:

```sql
@dba: dba
```

Then any PAM user in the PAM group `dba` would be mapped to the MariaDB user account `dba`. But if a PAM user with the name `dba` did not also exist, then the `pam_user_map` PAM module's debug logging would write errors to the syslog like the following:

```sql
Sep 27 17:17:05 dbserver1 mysqld: pam_user_map(mysql:auth): Opening file '/etc/security/user_map.conf'.
Sep 27 17:17:05 dbserver1 mysqld: pam_user_map(mysql:auth): Incoming username 'alice'.
Sep 27 17:17:05 dbserver1 mysqld: pam_user_map(mysql:auth): User belongs to 4 groups [dba,mongod,mongodba,mysql].
Sep 27 17:17:05 dbserver1 mysqld: pam_user_map(mysql:auth): Check if user is in group 'mysql': YES
Sep 27 17:17:05 dbserver1 mysqld: pam_user_map(mysql:auth): User mapped as 'dba'
Sep 27 17:17:05 dbserver1 mysqld: pam_unix(mysql:account): could not identify user (from getpwnam(dba))
Sep 27 17:17:05 dbserver1 mysqld: pam_sss(mysql:account): Access denied for user dba: 10 (User not known to the underlying authentication module)
Sep 27 17:17:05 dbserver1 mysqld: 2018-09-27 17:17:05 72 [Warning] Access denied for user 'alice'@'localhost' (using password: NO)
```

In the above log snippet, notice that both the `pam_unix` and the `pam_sss` PAM modules are complaining that the `dba` PAM user does not appear to exist, and that these complaints cause the PAM authentication process to fail, which causes the MariaDB authentication process to fail as well.

This can be fixed by creating a PAM user with the same name as the mapped MariaDB user account, which is `dba` in this case.

You may also be able to work around this problem by essentially disabling PAM's account verification for the service with the <a undefined>pam_permit</a> PAM module. For example, in the above case, that would be:

```sql
auth required pam_sss.so
auth required pam_user_map.so debug
account required pam_permit.so
```

See [MDEV-17315](https://jira.mariadb.org/browse/MDEV-17315) for more information.

## Tutorials

You may find the following PAM and user mapping-related tutorials helpful:

- [Configuring PAM Authentication and User Mapping with Unix Authentication](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-with-pluggable-authentication-modules-pam/configuring-pam-authentication-and-user-mapping-with-unix-authentication)
- [Configuring PAM Authentication and User Mapping with LDAP Authentication](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-with-pluggable-authentication-modules-pam/configuring-pam-authentication-and-user-mapping-with-ldap-authentication)

## See Also

- [Configuring PAM Authentication and User Mapping with MariaDB](https://mariadb.com/resources/blog/configuring-pam-authentication-and-user-mapping-with-mariadb/)
- [Configuring PAM Group Mapping with MariaDB](https://mariadb.com/resources/blog/configuring-pam-group-mapping-with-mariadb/)
- [Configuring LDAP Authentication and Group Mapping With MariaDB](http://www.geoffmontee.com/configuring-ldap-authentication-and-group-mapping-with-mariadb/)