# SHOW SLAVE HOSTS

## Syntax

```sql
SHOW SLAVE HOSTS
SHOW REPLICA HOSTS -- from MariaDB 10.5.1
```

## Description

This command is run on the primary and displays a list of replicas that are currently registered with it. Only replicas started with the <code class="highlight fixed" style="white-space:pre-wrap">--report-host=host_name</code> option
are visible in this list.

The list is displayed on any server (not just the primary server). The output
looks like this:

```sql
SHOW SLAVE HOSTS;
+------------+-----------+------+-----------+
| Server_id  | Host      | Port | Master_id |
+------------+-----------+------+-----------+
|  192168010 | iconnect2 | 3306 | 192168011 |
| 1921680101 | athena    | 3306 | 192168011 |
+------------+-----------+------+-----------+
```

- <strong>`Server_id`</strong>: The unique server ID of the replica server, as configured in the server's option file, or on the command line with [--server-id=value](/kb/en/replication-and-binary-log-server-system-variables/#server_id).
- <strong>`Host`</strong>: The host name of the replica server, as configured in the server's option file, or on the command line with `--report-host=host_name`. Note that this can differ from the machine name as configured in the operating system.
- <strong>`Port`</strong>: The port the replica server is listening on.
- <strong>`Master_id`</strong>: The unique server ID of the primary server that the replica server is replicating from.

Some MariaDB and MySQL versions report another variable, [rpl_recovery_rank](/kb/en/server-system-variables/#rpl_recovery_rank). This
variable was never used, and was eventually removed in [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/) .

Requires the [REPLICATION MASTER ADMIN](/kb/en/grant/#replication-master-admin) privilege (&gt;= [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)) or the [REPLICATION SLAVE](/kb/en/grant/#replication-slave) privilege (&lt;= [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/)).

#### SHOW REPLICA HOSTS

##### MariaDB starting with [10.5.1](/kb/en/mariadb-1051-release-notes/)

`SHOW REPLICA HOSTS` is an alias for `SHOW SLAVE HOSTS` from [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/).

## See Also

- [MariaDB replication](/kb/en/high-availability-performance-tuning-mariadb-replication/)
- [Replication threads](/replication/standard-replication/replication-threads)
- [SHOW PROCESSLIST](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-processlist). In [SHOW PROCESSLIST](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-processlist) output, replica threads are identified by `Binlog Dump`