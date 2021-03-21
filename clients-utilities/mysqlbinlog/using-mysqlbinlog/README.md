# Using mysqlbinlog

##### MariaDB starting with [10.4.6](/kb/en/mariadb-1046-release-notes/)

From [MariaDB 10.4.6](/kb/en/mariadb-1046-release-notes/), `mariadb-binlog` is a symlink to `mysqlbinlog`.

##### MariaDB starting with [10.5.2](/kb/en/mariadb-1052-release-notes/)

From [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), `mariadb-binlog` is the name of the tool, with `mysqlbinlog` a symlink .

The MariaDB server's [binary log](/mariadb-administration/server-monitoring-logs/binary-log) is a set of files containing "events" which represent modifications to the contents of a MariaDB database. These events are written in a binary (i.e. non-human-readable) format. The <em>mysqlbinlog</em> utility is used to view these events in plain text.

Run [mysqlbinlog](/clients-utilities/mysqlbinlog) from a command-line like this:

```sql
shell> mysqlbinlog [options] log_file ...
```

See [mysqlbinlog Options](/clients-utilities/mysqlbinlog/mysqlbinlog-options) for details on the available options.

As an example, here is how you could display the contents of a [binary log](/mariadb-administration/server-monitoring-logs/binary-log) file
named "mariadb-bin.000152":

```sql
shell> mysqlbinlog mariadb-bin.000152
```

If you are using statement-based logging (the default) the output includes the
SQL statement, the ID of the server the statement was executed on, a timestamp,
and how much time the statement took to execute. If you are using row-based
logging the output of an event will not include an SQL statement but will
instead output how individual rows were changed.

The output from mysqlbinlog can be used as input to the mysql client to redo
the statements contained in a [binary log](/mariadb-administration/server-monitoring-logs/binary-log). This is useful for recovering after a server crash. Here is an example:

```sql
shell> mysqlbinlog binlog-filenames | mysql -u root -p
```

If you would like to view and possibly edit the file before applying it to your
database, use the '-r' flag to redirect the output to a file:

```sql
shell> mysqlbinlog -r filename binlog-filenames
```

You can then open the file and view it and delete any statements you don't want
executed (such as an accidental DROP DATABASE). Once you are satisfied with the
contents you can execute it with:

```sql
shell> mysql -u root -p < filename
```

Be careful to process multiple log files in a single connection, especially if
one or more of them have any <code class="fixed" style="white-space:pre-wrap">CREATE TEMPORARY TABLE ...</code>
statements. Temporary tables are dropped when the mysql client terminates, so
if you are processing multiple log files one at a time (i.e. multiple
connections) and one log file creates a temporary table and then a subsequent
log file refers to the table you will get an 'unknown table' error.

To execute multiple logfiles using a single connection, list them all on the
mysqlbinlog command line:

```sql
shell> mysqlbinlog mariadb-bin.000001 mariadb-bin.000002 | mysql -u root -p
```

If you need to manually edit the binlogs before executing them, combine them
all into a single file before processing. Here is an example:

```sql
shell> mysqlbinlog mariadb-bin.000001 > /tmp/mariadb-bin.sql
shell> mysqlbinlog mariadb-bin.000002 >> /tmp/mariadb-bin.sql
shell> # make any edits
shell> mysql -u root -p -e "source /tmp/mariadb-bin.sql"
```

## See Also

- [mysqlbinlog](/clients-utilities/mysqlbinlog)
- [mysqlbinlog Options](/clients-utilities/mysqlbinlog/mysqlbinlog-options)