# RESET MASTER

```sql
RESET MASTER [TO #]
```

Deletes all [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) files listed in the index file, resets the
binary log index file to be empty, and creates a new binary log file with a suffix of .000001.

##### MariaDB starting with [10.1.6](/kb/en/mariadb-1016-release-notes/)

If <code class="fixed" style="white-space:pre-wrap">TO #</code> is given, then the first new binary log file will start from number #.

This statement is for use only when the master is started for the first time, and should never be used if any slaves are actively [replicating](/replication/) from the binary log.

## See Also

- The [PURGE BINARY LOGS](/kb/en/sql-commands-purge-logs/) statement is intended for use in active replication.