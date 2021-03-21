# Activating the Binary Log

To enable binary logging, start the server with the <a undefined>--log-bin[=name</a>] option.

If you specify a filename with an extension (for example `.log`), the extension will be silently ignored.

If you don't provide a name (which can, optionally, include an absolute path), the default will be `datadir/log-basename-bin`, `datadir/mysql-bin` or `datadir/mariadb-bin` (the latter two if [--log-basename](/kb/en/mysqld-options-full-list/#-log-basename) is not specified, and dependent on server version). Datadir is determined by the value of the [datadir](/kb/en/server-system-variables/#datadir) system variable.

We strongly recommend you use either [--log-basename](/kb/en/mysqld-options-full-list/#-log-basename) or specify a filename to ensure that [replication](/replication/) doesn't stop if the hostname of the computer changes.

The directory storing the binary logs will contain a binary log index, as well as the individual binary log files.

The binary log files will have a series of numbers as filename extensions. Each additional binary log will increment the extension number, so the oldest binary logs will have lower numbers, the most recent, higher numbers.

A new binary log, with a new extension, is created every time the server starts, the logs are flushed, or the maximum size is reached (determined by [max_binlog_size](/kb/en/server-system-variables/#max_binlog_size)).

The binary log index file contains a master list of all the binary logs, in order.

A sample listing from a directory containing the binary logs:

```sql
shell> ls -l 
total 100
...
-rw-rw---- 1 mysql adm 2098 Apr 19 00:46 mariadb-bin.000079
-rw-rw---- 1 mysql adm  332 Apr 19 00:56 mariadb-bin.000080
-rw-rw---- 1 mysql adm  347 Apr 19 07:36 mariadb-bin.000081
-rw-rw---- 1 mysql adm  306 Apr 20 07:15 mariadb-bin.000082
-rw-rw---- 1 mysql adm  332 Apr 20 07:41 mariadb-bin.000083
-rw-rw---- 1 mysql adm  373 Apr 21 07:56 mariadb-bin.000084
-rw-rw---- 1 mysql adm  347 Apr 21 09:09 mariadb-bin.000085
-rw-rw---- 1 mysql adm  398 Apr 21 21:24 mariadb-bin.000086
-rw-rw---- 1 mysql adm  816 Apr 21 17:05 mariadb-bin.index
```

The binary log index file will by default have the same name as the individual binary logs, with the extension .index. You can specify an alternative name with the `--log-bin-index[=filename]` [option](/kb/en/mysqld-options-full-list/).

Clients with the SUPER privilege can disable and re-enable the binary log for the current session by setting the [sql_log_bin](/kb/en/replication-and-binary-log-server-system-variables/#sql_log_bin) variable.

```sql
SET sql_log_bin = 0;

SET sql_log_bin = 1;
```

## Binary Log Format

There are three formats for the binary log. The default is statement-based logging, while row-based logging and a mix of the two formats are also possible. See [Binary Log Formats](/mariadb-administration/server-monitoring-logs/binary-log/binary-log-formats/) for a full discussion.

## See Also

- [Setting sql_log_bin](/sql-statements-structure/sql-statements/administrative-sql-statements/set-commands/set-sql_log_bin/)
- [PURGE LOGS](/kb/en/sql-commands-purge-logs/) - Delete logs
- [FLUSH LOGS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/)  - Close and rotate logs