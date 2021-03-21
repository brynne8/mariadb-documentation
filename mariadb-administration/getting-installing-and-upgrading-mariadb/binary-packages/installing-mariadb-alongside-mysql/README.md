# Installing MariaDB Alongside MySQL

MariaDB was designed as a drop-in replacement of MySQL, with more features, new storage engines, fewer bugs, and better performance, but you can also install it alongside MySQL. (This can be useful, for example, if you want to migrate
databases/applications one by one.)

Here are the steps to install MariaDB near an existing MySQL installation.

- Download the compiled binary tar.gz that contains the latest version
  ([mariadb-5.5.24-linux-x86_64.tar.gz](http://downloads.mariadb.org/mariadb/5.5.24/#bits=64&amp;file_type=tar_gz)
  - as of writing this article) and extract the files in a directory of your
  choice. I will assume for this article that the directory was <strong>/opt</strong>.

```sql
[root@mariadb-near-mysql ~]# cat /etc/issue
CentOS release 6.2 (Final)

[root@mariadb-near-mysql ~]# rpm -qa mysql*
mysql-5.1.61-1.el6_2.1.x86_64
mysql-libs-5.1.61-1.el6_2.1.x86_64
mysql-server-5.1.61-1.el6_2.1.x86_64

[root@mariadb-near-mysql ~]# ps axf | grep mysqld
 2072 pts/0    S+     0:00          \_ grep mysqld
 1867 ?        S      0:01 /bin/sh /usr/bin/mysqld_safe --datadir=/var/lib/mysql --socket=/var/lib/mysql/mysql.sock ...
 1974 ?        Sl     0:06  \_ /usr/libexec/mysqld --basedir=/usr --datadir=/var/lib/mysql --user=mysql ...
```

- Create data directory and symlinks as below:

```sql
[root@mariadb-near-mysql opt]# mkdir mariadb-data
[root@mariadb-near-mysql opt]# ln -s mariadb-5.5.24-linux-x86_64 mariadb
[root@mariadb-near-mysql opt]# ls -al
total 20
drwxr-xr-x.  5 root root 4096 2012-06-06 07:27 .
dr-xr-xr-x. 23 root root 4096 2012-06-06 06:38 ..
lrwxrwxrwx.  1 root root   27 2012-06-06 07:27 mariadb -> mariadb-5.5.24-linux-x86_64
drwxr-xr-x. 13 root root 4096 2012-06-06 07:07 mariadb-5.5.24-linux-x86_64
drwxr-xr-x.  2 root root 4096 2012-06-06 07:26 mariadb-data
```

- Create group <strong>mariadb</strong> and user <strong>mariadb</strong> and set correct ownerships:

```sql
[root@mariadb-near-mysql opt]# groupadd --system mariadb
[root@mariadb-near-mysql opt]# useradd -c "MariaDB Server" -d /opt/mariadb -g mariadb --system mariadb
[root@mariadb-near-mysql opt]# chown -R mariadb:mariadb mariadb-5.5.24-linux-x86_64/
[root@mariadb-near-mysql opt]# chown -R mariadb:mariadb mariadb-data/
```

- Create a new <strong>my.cnf</strong> in <strong>/opt/mariadb</strong> from support files:

```sql
[root@mariadb-near-mysql opt]# cp mariadb/support-files/my-medium.cnf mariadb-data/my.cnf
[root@mariadb-near-mysql opt]# chown mariadb:mariadb mariadb-data/my.cnf
```

- Edit the file <strong>/opt/mariadb-data/my.cnf</strong> and add custom paths, socket, port, user and the most important of all: data directory and base directory. Finally the file should have at least the following:

```sql
[client]
port		= 3307
socket		= /opt/mariadb-data/mariadb.sock

[mysqld]
datadir         = /opt/mariadb-data
basedir         = /opt/mariadb
port		= 3307
socket		= /opt/mariadb-data/mariadb.sock
user            = mariadb
```

- Copy the init.d script from support files in the right location:

```sql
[root@mariadb-near-mysql opt]# cp mariadb/support-files/mysql.server /etc/init.d/mariadb
[root@mariadb-near-mysql opt]# chmod +x /etc/init.d/mariadb
```

- Edit <strong>/etc/init.d/mariadb</strong> replacing <strong>mysql</strong> with <strong>mariadb</strong> as below:

```sql
- # Provides: mysql
+ # Provides: mariadb
- basedir=
+ basedir=/opt/mariadb
- datadir=
+ datadir=/opt/mariadb-data
- lock_file_path="$lockdir/mysql"
+ lock_file_path="$lockdir/mariadb"
```

The trickiest part will be the last changes to this file. You need to tell
mariadb to use only one cnf file. In the <strong>start</strong> section after
<strong>$bindir/mysqld_safe</strong> add <strong>--defaults-file=/opt/mariadb-data/my.cnf</strong>.
Finally the lines should look like:

```sql
# Give extra arguments to mysqld with the my.cnf file. This script
# may be overwritten at next upgrade.
$bindir/mysqld_safe --defaults-file=/opt/mariadb-data/my.cnf --datadir="$datadir" --pid-file="$mysqld_pid_file_path" $other_args >/dev/null 2>&1 &
```

The same change needs to be made to the mysqladmin command below in the wait_for_ready() function so that the mariadb start command can properly listen for the server start. In the <strong>wait_for_ready()</strong> function, after <strong>$bindir/mysqladmin</strong> add <strong>--defaults-file=/opt/mariadb-data/my.cnf</strong>.
The lines should look like:

```sql
wait_for_ready () {
[...]
    if $bindir/mysqladmin --defaults-file=/opt/mariadb-data/my.cnf ping >/dev/null 2>&1; then
```

- Run <strong>mysql_install_db</strong> by explicitly giving it the my.cnf file as argument:

```sql
[root@mariadb-near-mysql opt]# cd mariadb
[root@mariadb-near-mysql mariadb]# scripts/mysql_install_db --defaults-file=/opt/mariadb-data/my.cnf
```

- Now you can start MariaDB by

```sql
[root@mariadb-near-mysql opt]# /etc/init.d/mariadb start
Starting MySQL...                                          [  OK  ]
```

- Make MariaDB start at system start:

```sql
[root@mariadb-near-mysql opt]# cd /etc/init.d
[root@mariadb-near-mysql init.d]# chkconfig --add mariadb 
[root@mariadb-near-mysql init.d]# chkconfig --levels 3 mariadb on
```

- Finally test that you have both instances running:

```sql
[root@mariadb-near-mysql ~]# mysql -e "SELECT VERSION();"
+-----------+
| VERSION() |
+-----------+
| 5.1.61    |
+-----------+
[root@mariadb-near-mysql ~]# mysql -e "SELECT VERSION();" --socket=/opt/mariadb-data/mariadb.sock
+----------------+
| VERSION()      |
+----------------+
| 5.5.24-MariaDB |
+----------------+
```

## What about MariaDB Upgrades ?

By having the <strong>mariadb.socket</strong>, <strong>my.cnf</strong> file and <strong>databases</strong> in <strong>/opt/mariadb-data</strong> if you want to upgrade the MariaDB version you will will only need to:

- extract the new version from the archive in <strong>/opt</strong> near the current version
- stop MariaDB
- change the symlink <strong>mariadb</strong> to point to the new directory
- start MariaDB
- run upgrade script... but remember you will need to provide the socket option <strong>--socket=/opt/mariadb-data/mariadb.sock</strong>