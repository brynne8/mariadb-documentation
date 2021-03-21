# MASTER_POS_WAIT

##### MariaDB starting with [10.0.9](/kb/en/mariadb-1009-release-notes/)

MASTER_POS_WAIT was introduced in [MariaDB 10.0.9](/kb/en/mariadb-1009-release-notes/).

## Syntax

```sql
MASTER_POS_WAIT(log_name,log_pos[,timeout,["connection_name"]])
```

## Description

This function is useful in [replication](/replication/) for controlling master/slave synchronization.  It blocks until the slave has read and applied all updates up to the specified position (`log_name,log_pos`) in the master log. The return value is the number of log events the slave had to wait for to advance to the specified position. The function returns NULL if
the slave SQL thread is not started, the slave's master information is not
initialized, the arguments are incorrect, or an error occurs. It returns -1 if
the timeout has been exceeded. If the slave SQL thread stops while
 <code class="highlight fixed" style="white-space:pre-wrap">MASTER_POS_WAIT()</code> is waiting, the function returns NULL. If
the slave is past the specified position, the function returns immediately.

If a `timeout` value is specified, <code class="highlight fixed" style="white-space:pre-wrap">MASTER_POS_WAIT()</code> stops
waiting when `timeout` seconds have elapsed. `timeout` must be greater than 0; a
zero or negative `timeout` means no `timeout`.

The <code class="highlight fixed" style="white-space:pre-wrap">connection_name</code> is used when you are using [multi-source-replication](/replication/standard-replication/multi-source-replication/).  If you don't specify it, it's set to the value of the [default_master_connection](/kb/en/replication-and-binary-log-server-system-variables/#default_master_connection) system variable.

Statements using the MASTER_POS_WAIT() function are not [safe for replication](/kb/en/unsafe-statements-for-replication/).