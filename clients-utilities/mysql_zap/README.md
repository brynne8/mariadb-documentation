# mysql_zap

##### MariaDB until [10.1](/kb/en/what-is-mariadb-101/)

mysql_zap was removed in [MariaDB 10.2](/kb/en/what-is-mariadb-102/). pkill can be used  [as an alternative](#pkill-as-an-alternative).

<em>mysql_zap</em> kills processes that match a pattern. It uses the <em>ps</em> command and Unix signals, so it runs on Unix and Unix-like systems.

Invoke mysql_zap like this:

```sql
shell> mysql_zap [-signal] [-?Ift] 
```

A process matches if its output line from the <em>ps</em> command contains the pattern. By default, mysql_zap asks for confirmation for each process. Respond <em>y</em> to kill the process, or <em>q</em> to exit mysql_zap. For any other response, mysql_zap does not attempt to kill the process.

If the <em>-signal</em> option is given, it specifies the name or number of the signal to send to each
process. Otherwise, mysql_zap tries first with TERM (signal 15) and then with KILL (signal 9).

mysql_zap supports the following additional options:

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>--help</code>, <code>-?</code>, <code>-I</code></td><td>Display a help message and exit.</td></tr>
<tr><td><code>-f</code></td><td>Force mode. <em>mysql_zap</em> attempts to kill each process without confirmation.</td></tr>
<tr><td><code>-t</code></td><td>Test mode. Display information about each process but do not kill it.</td></tr>
</tbody></table>

## Example

```sql
localhost:~# mysql_zap -t mysql
stty: standard input: unable to perform all requested operations
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root      4073  0.0  0.2   3804  1308 ?        S    08:51   0:00 /bin/bash /usr/bin/mysqld_safe
mysql     4258  3.3 15.7 939740 81236 ?        Sl   08:51  30:18 /usr/sbin/mysqld --basedir=/usr --datadir=/var/lib/mysql --user=mysql --pid-file=/var/run/mysqld/mysqld.pid --socket=/var/run/mysqld/mysqld.sock --port=3306
```

## pkill as an Alternative

<em>pkill</em> can be used as an alternative to <em>mysql_zap</em>, although an important distinction between pkill and mysql_zap is that mysql_zap kills the server 'gently' first (with signal 15) and only if the server doesn't die in a limited time then tries -9.

To use pkill in the same way, one must run it twice; `pkill --signal 15 mysqld ; sleep(10) ; pkill -f --signal 9 pattern`