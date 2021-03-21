# ColumnStore Database User Management

# Basic user management

MariaDB ColumnStore allows permissions to be set for user accounts. The syntax of these grants follows the standard MariaDB syntax (see [GRANT](/sql-statements-structure/sql-statements/account-management-sql-commands/grant)).

For the root user, ColumnStore comes with full privileges. In order to set/restrict user accounts, privileges must be given/restricted. ColumnStore uses a dedicated schema called <em>infinidb_vtable</em> for creation of all temporary tables used for ColumnStore query processing. The root user account has been given permission to this account by default, but full permission MUST be given for all user accounts to this schema:

```sql
GRANT CREATE TEMPORARY TABLES ON infinidb_vtable.* TO user_account; 
```

where user_account = user login, server and password characteristics

Further permissions/restrictions can now be placed on any existing objects (tables, functions, procedures, views) for any access/limitations wanting to be placed on users:
Example to give a user that has a password full access to all tables for a database (after the above grant has been given):

```sql
USE mysql;
GRANT ALL ON my_schema.* TO ‘someuser’@’somehost’
IDENTIFIED BY ‘somepassword’;
FLUSH PRIVILEGES;
```

Example to give a user that has a password read-only access to only 1 table (after the above grant has been given):

```sql
USE mysql;
GRANT SELECT ON my_schema.table1 TO ‘someuser’@’somehost’
IDENTIFIED BY ‘somepassword’;
FLUSH PRIVILEGES;
```

# PAM Unix authentication

Starting with ColumnStore 1.0.8, ColumnStore includes the necessary authentication plugin for PAM support. For general details see [pam-authentication-plugin](/kb/en/pam-authentication-plugin/) but here we will outline the steps necessary to configure this for os authentication specific to a ColumnStore installation.

First ensure that the mysql user has read access to the /etc/shadow file, in this example a group is used to facilitate this:

```sql
$ sudo groupadd shadow 
$ sudo usermod -a -G shadow mysql 
$ sudo chown root:shadow /etc/shadow 
$ sudo chmod g+r /etc/shadow
```

Create a pam.d entry to configure unix password authentication:

```sql
$ vi /etc/pam.d/mysql
auth required pam_unix.so
account required pam_unix.so
```

Load the auth_pam.so plugin and create a user:

```sql
$ mcsmysql
> INSTALL SONAME 'auth_pam';
> GRANT SELECT ON test.* TO david IDENTIFIED VIA pam;
> GRANT CREATE TEMPORARY TABLE ON infinidb_vtable.* TO david;
```

Restart ColumnStore so that the mariadb server process picks up the auth plugin and group changes:

```sql
$ sudo su - 
$ mcsadmin restartSystem
```

Now attempt to login to verify correct setup, entering the unix password for the account david when prompted:

```sql
$ mcsmysql -u david -p
```

If this still fails, try restartSystem once more and try logging in again as this seems to resolve the issue.

# PAM LDAP authentication

Follow the instructions above for Pam UNIX authentication with the exception of the pam.d mysql file:

```sql
$ vi /etc/pam.d/mysql
auth required pam_ldap.so
account required pam_ldap.so
```

# PAM LDAP group authentication

The PAM plugin can also be utilized for LDAP group authentication. A good reference written by a MariaDB support engineer on setting up OpenLDAP can be found [here](http://www.geoffmontee.com/configuring-ldap-authentication-and-group-mapping-with-mariadb/). Some additional notes that may be of help:

- if you get an error with the directory manager script, there are a couple of lines that may be wrapped if you cut and paste. Each line in the script must start with a command then a colon then a space.
- 'authconfig --disableldapauth --disableldap --enableshadow --updateall' can be run to remove the ldap auth should you want to do this or if you made a mistake in the configuration values that you want to correct by re-running the command.

This example assumes you have followed the initial instructions in that article and setup an LDAP user 'geoff' in group 'mysql-admins' in domain ' dc=support,dc=mariadb'. The following instructions are adapted to reflect ColumnStore and should replace the latter MariaDB setup of the blog.

The PAM user mapping library must be built and installed locally:

```sql
wget https://raw.githubusercontent.com/MariaDB/server/10.1/plugin/auth_pam/mapper/pam_user_map.c
gcc pam_user_map.c -shared -lpam -fPIC -o pam_user_map.so
sudo install --mode=0755 pam_user_map.so /lib64/security/
```

Ensure that the 'mysql' user has read access to the /etc/shadow file, in this example a group is used to facilitate this:

```sql
$ sudo groupadd shadow 
$ sudo usermod -a -G shadow mysql 
$ sudo chown root:shadow /etc/shadow 
$ sudo chmod g+r /etc/shadow
```

Create a pam.d entry to configure ldap password authentication:

```sql
$ sudo vi /etc/pam.d/mysql
#%PAM-1.0
auth sufficient pam_ldap.so use_first_pass
auth sufficient pam_unix.so nullok try_first_pass
auth required pam_user_map.so

account [default=bad success=ok user_unknown=ignore] pam_ldap.so
account required pam_unix.so broken_shadow
```

Next the user mapping must be configured, this will map members of the ldap group 'mysql-admins' to the local 'dba' account (the @ character indicates that mysql-admins is a group):

```sql
$ sudo vi /etc/security/user_map.conf
@mysql-admins: dba
```

As an alternative, specific accounts can be referenced as follows.

```sql
$ sudo vi /etc/security/user_map.conf
geoff: dba
```

Due to the way that PAM works, a local 'dba' account must exist:

```sql
sudo useradd dba
```

Next, the pam plugin and mariadb accounts can be configured:

```sql
$ mcsmysql
-- Install the plugin
INSTALL SONAME 'auth_pam';

-- remove standard anonymous users if existing
DELETE FROM mysql.user WHERE User='';
FLUSH PRIVILEGES;

-- Create the "dba" user
CREATE USER 'dba'@'localhost' IDENTIFIED BY 'strongpassword';
GRANT ALL PRIVILEGES ON *.* TO 'dba'@'localhost';

-- Create an anonymous catch-all user that will use the PAM plugin and the mysql PAM policy
CREATE USER ''@'localhost' IDENTIFIED VIA pam USING 'mysql';

-- Allow the anonymous user to proxy as the dba user
GRANT PROXY ON 'dba'@'localhost' TO ''@'localhost';

-- columnstore temp table permission grant
GRANT CREATE TEMPORARY TABLE ON infinidb_vtable.* TO ''@'localhost' IDENTIFIED VIA pam;
```

Now restart columnstore since the user group permissions of mysql have changed and need to be picked up by the mysqld process:

```sql
mcsadmin restartSystem 
```

Test that an ldap account can now authenticate:

```sql
$ mcsmysql -u geoff -h localhost
[mariadb] Password:
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 10
Server version: 10.1.23-MariaDB Columnstore 1.0.x-1

Copyright (c) 2000, 2017, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> SELECT USER(), CURRENT_USER();
+-----------------+----------------+
| USER()          | CURRENT_USER() |
+-----------------+----------------+
| geoff@localhost | dba@localhost  |
+-----------------+----------------+
1 row in set (0.00 sec)

MariaDB [(none)]> select c1 from test.test_cs;
+------+
| c1   |
+------+
|    1 |
+------+
1 row in set (0.03 sec)
```

The first query shows that the authenticated user is 'geoff@localhost' while current user is 'dba@localhost'. This shows that the session authenticated as geoff and proxied to dba correctly.

The second query just tests that a columnstore table can be queried correctly and should be updated for your local schema as appropriate.

# User Resource Allocation

MariaDB ColumnStore supports the ability to give priority to resources allocated (CPU) based on a user. Users are allocated at least the % of CPU that they are assigned to by priority setting.
Effectively a particular user or a set of users can be guaranteed a set amount of resources. E.g:

- User 1 gets a minimum of 40% CPU Resources
- User 2 gets a minimum of 30% CPU Resources
- If any user logs in for a query while User 1 and User 2 are running queries, these new users (i.e., User 3,4 and 5) get only the remaining 30% of the CPU Resources."

## User Priority Management

Three stored procedures were created in the <em>infinidb_querystats</em> schema for the user to set, remove and view user priorities. The priority table associates a user with a priority level. A user that does not have an entry is given the low priority level by default.

```sql
CalSetUserPriority (host varchar, user varchar, priority varchar)
```

- Assigns a priority level to user@host.
- Priority is case insensitive 'high', 'medium' or 'low'.
- Host and user will be validated to exist in MariaDB

```sql
CalRemoveUserPriority(host varchar, user varchar)
```

- Removes the user entry, effectively restoring the default of 'low'.
- User existence will not be validated.

```sql
CalShowProcessList()
```

- Prints a combination of mariadb 'show processlist' and user priority

The MariaDB user needs to be granted the execute privileges for these procedures and the select privileges for the tables in the <em>infinidb_querystats</em> schema. Or, chances are, the following should just work for a super user:

```sql
GRANT ALL ON infinidb_querystats.* TO 'user'@'host'; // user will now have the privilege to use the priority procedures and view query stats.
```

### Enabling User Priority

To enable this feature, the &lt;UserPriority&gt;&lt;Enabled&gt; element in the MariaDB ColumnStore configuration file  should be set to Y (default is N).

```sql
<UserPriority>
     <Enabled>Y</Enabled>
</UserPriority>
```

Cross Engine Support must also be enabled. See the ”Cross-Engine Table Access” section in this guide.

## User Priority Processing

The PrimProc process has one job queue for each priority level and thread assigned to each queue. The number of threads assigned to each queue is configurable using the following elements in the configuration file:

```sql
<PrimitiveServer><HighPriorityPercentage>
<PrimitiveServer><MediumPriorityPercentage>
<PrimitiveServer><LowPriorityPercentage>
```

The defaults are 60, 30, and 10 respectively. Each queue is given at least 1 thread so there is neither 'idle' priority configuration possible nor starvation. The number of threads started is normalized such that 100% = 2 * (the number of cores on the machine). The user can overbook or underbook their CPUs however they want.
This is an example of how threads are assigned on an 8-core system using the defaults.

- 10% of 16 = 1.6, rounds down to 1 thread for the low priority queue.
- 30% of 16 = 4.8, rounds down to 4 threads for the medium priority queue.
- The high priority queue gets the remaining 11 threads.

Each thread is given a preferred queue to get work from. If a thread's preferred queue is empty, it will choose jobs from the high, then medium, then low priority queues. If there are only low priority jobs running, on an 8-core system all 16 threads will process jobs from the low priority queue. If a medium priority query starts, using the defaults, the 15 threads assigned to the high and medium queues will process the medium queue, leaving the 1 assigned to the low queue to process the low priority jobs. Then, if a high priority query starts, the 11 threads assigned to the high priority queue will begin processing the high priority jobs, the 4 assigned to the medium queue will process those jobs, and the 1 assigned to the low queue will process those jobs. <br>
Given this algorithm, the configuration parameters could be thought of as minimum levels for each priority.
<br>
Note that this implementation only affects the processing done by PrimProc. Depending on the work distribution of a given query, a user may or may not observe overall performance proportional to their priority level.