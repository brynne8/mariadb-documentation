# Relay Log

The relay log is a set of log files created by a replica during [replication](/replication/standard-replication/).

It's the same format as the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/), containing a record of events that affect the data or structure; thus, [mysqlbinlog](/clients-utilities/mysqlbinlog/) can be used to display its contents. It consists of a set of relay log files and an index file containing a list of all relay log files.

Events are read from the primary's binary log and written to the replica's relay log. They are then performed on the replica. Old relay log files are automatically removed once they are no longer needed.

## Creating Relay Log Files

New relay log files are created by the replica at the following times:

- when the IO thread starts
- when the logs are flushed, with [FLUSH LOGS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/) or [mysqladmin flush-logs](/clients-utilities/mysqladmin/).
- when the maximum size, determined by the [max_relay_log_size](/kb/en/replication-and-binary-log-server-system-variables/#max_relay_log_size) system variable, has been reached

## Relay Log Names

By default, the relay log will be given a name `host_name-relay-bin.nnnnnn`, with `host_name` referring to the server's host name, and #nnnnnn` the sequence number.`

This will cause problems if the replica's host name changes, returning the error `Failed to open the relay log` and `Could not find target log during relay log initialization`. To prevent this, you can specify the relay log file name by setting the [relay_log](/kb/en/replication-and-binary-log-server-system-variables/#relay_log) and [relay_log_index](/kb/en/replication-and-binary-log-server-system-variables/#relay_log_index) system variables.

If you need to overcome this issue while replication is already underway,you can stop the replica, prepend the old relay log index file to the new relay log index file, and restart the replica.

For example:

```sql
shell> cat NEW_relay_log_name.index >> OLD_relay_log_name.index
shell> mv NEW_relay_log_name.index OLD_relay_log_name.index
```

## Viewing Relay Logs

The [SHOW RELAYLOG EVENTS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-relaylog-events/) shows events in the relay log, and, since relay log files are the same format as binary log files, they can be read with the [mysqlbinlog](/clients-utilities/mysqlbinlog/) utility.

## Removing Old Relay Logs

Old relay logs are automatically removed once all events have been implemented on the replica, and the relay log file is no longer needed. This behavior can be changed by adjusting the [relay_log_purge](/kb/en/replication-and-binary-log-server-system-variables/#relay_log_purge) system variable from its default of `1` to `0`, in which case the relay logs will be left on the server.

If the relay logs are taking up too much space on the replica, the [relay_log_space_limit](/kb/en/replication-and-binary-log-server-system-variables/#relay_log_space_limit) system variable can be set to limit the size. The IO thread will stop until the SQL thread has cleared the backlog. By default there is no limit.