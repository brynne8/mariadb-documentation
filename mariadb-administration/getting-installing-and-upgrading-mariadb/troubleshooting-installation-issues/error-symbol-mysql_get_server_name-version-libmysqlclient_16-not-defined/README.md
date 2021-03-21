# Error: symbol mysql_get_server_name, version libmysqlclient_16 not defined

If you see the error message:

```sql
symbol mysql_get_server_name, version libmysqlclient_16 not defined in file libmysqlclient.so.16 with link time reference
```

...then you are probably trying to use the mysql command-line client from
MariaDB with libmysqlclient.so from MySQL.

The symbol <code class="highlight fixed" style="white-space:pre-wrap">mysql_get_server_name()</code> is something present in the MariaDB
source tree and not in the MySQL tree.

If you have both the MariaDB client package and the MySQL client packages
installed this error will happen if your system finds the MySQL version of
<code class="highlight fixed" style="white-space:pre-wrap">libmysqlclient.so</code> first.

To figure out which library is being linked in dynamically (ie, the
wrong one) use the 'ldd' tool.

```sql
ldd $(which mysql) | grep mysql
```

or

```sql
ldd /path/to/the/binary | grep mysql
```

For example:

```sql
me@mybox:~$ ldd $(which mysql)|grep mysql
        libmysqlclient.so.16 => /usr/lib/libmysqlclient.so.16 (0xb74df000)
```

You can then use your package manager's tools to find out which package the library belongs to.

On CentOS the command to find out which package installed a specific file is:

```sql
rpm -qf /path/to/file
```

On Debian-based systems, the command is:

```sql
dpkg -S /path/to/file
```

Here's an example of locating the library and finding out which package it belongs to on an Ubuntu system:

```sql
me@mybox:~$ ldd $(which mysql)|grep mysql
	libmysqlclient.so.16 => /usr/lib/libmysqlclient.so.16 (0xb75f8000)
me@mybox:~$ dpkg -S /usr/lib/libmysqlclient.so.16
libmariadbclient16: /usr/lib/libmysqlclient.so.16
```

The above shows that the mysql command-line client is using the library
 <code class="highlight fixed" style="white-space:pre-wrap">/usr/lib/libmysqlclient.so.16</code> and that that library is part of
the <code class="highlight fixed" style="white-space:pre-wrap">libmariadbclient16</code> Ubuntu package. Unsurprisingly, the
mysql command-line client works perfectly on this system.

If the answer that came back had been something other than a MariaDB package,
then it is likely there would have been issues with running the MariaDB mysql client
application.

If the library that the system tries to use is not from a MariaDB
package, the remedy is to remove the offending package (and possibly install or re-install
the correct package) so that the correct library can be used.