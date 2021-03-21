# Thread Pool in MariaDB 5.1 - 5.3

This article describes the old thread pool in [MariaDB 5.1](/kb/en/what-is-mariadb-51/) - 5.3.

[MariaDB 5.5](/kb/en/what-is-mariadb-55/) and later use an improved thread pool - see  [Thread pool in MariaDB](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-pool/thread-pool-in-mariadb).

## About pool of threads

This is an extended version of the pool-of-threads code from MySQL 6.0.  This
allows you to use a limited set of threads to handle all queries, instead of
the old 'one-thread-per-connection' style. In recent times, its also been referred to as "thread pool" or "thread pooling" as this feature (in a different implementation) is available in Enterprise editions of MySQL (not in the Community edition).

This can be a very big win if most of your queries are short running queries
and there are few table/row locks in your system.

## Instructions

To enable pool-of-threads you must first run configure with the
<code class="highlight fixed" style="white-space:pre-wrap">--with-libevent</code> option. (This is automatically done if you
use any 'max' scripts in the BUILD directory):

```sql
./configure --with-libevent
```

When starting mysqld with the pool of threads code you should use

```sql
mysqld --thread-handling=pool-of-threads --thread-pool-size=20
```

Default values are:

```sql
thread-handling=  one-thread-per-connection
thread-pool-size= 20
```

One issue with pool-of-threads is that if all worker threads are doing
work (like running long queries) or are locked by a row/table lock no
new connections can be established and you can't login and find out
what's wrong or login and kill queries.

To help this, we have introduced two new options for mysqld; [extra_port](/kb/en/thread-pool-system-and-status-variables/#extra_port) and [extra_max_connections](/kb/en/thread-pool-system-and-status-variables/#extra_max_connections):

```sql
--extra-port=#             (Default 0)
--extra-max-connections=#  (Default 1)
```

If [extra-port](/kb/en/thread-pool-system-and-status-variables/#extra_port) is &lt;&gt; 0 then you can connect max_connections number of
normal threads + 1 extra SUPER user through the 'extra-port' TCP/IP
port.  These connections use the old one-thread-per-connection method.

To connect with through the extra port, use:

```sql
mysql --port='number-of-extra-port' --protocol=tcp
```

This allows you to freely use, on connection bases, the optimal
connection/thread model.

## See also

- [Thread-handling and thread-pool-size variables](/kb/en/thread-pool-system-and-status-variables/)
- [How MySQL Uses Threads for Client Connections](http://dev.mysql.com/doc/refman/5.6/en/connection-threads.html)