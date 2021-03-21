# mysqlbinlog

##### MariaDB starting with [10.4.6](/kb/en/mariadb-1046-release-notes/)

From [MariaDB 10.4.6](/kb/en/mariadb-1046-release-notes/), `mariadb-binlog` is a symlink to `mysqlbinlog`.

##### MariaDB starting with [10.5.2](/kb/en/mariadb-1052-release-notes/)

From [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), `mariadb-binlog` is the name of the tool, with `mysqlbinlog` a symlink .

`mysqlbinlog` is a utility included with MariaDB for processing [binary log](/mariadb-administration/server-monitoring-logs/binary-log) and [relay log](/mariadb-administration/server-monitoring-logs/binary-log/relay-log) files.

The MariaDB server's binary log is a set of files containing "events" which
represent modifications to the contents of a MariaDB database. These events are
written in a binary (i.e. non-human-readable) format. The mysqlbinlog utility
is used to view these events in plain text.

- [Using mysqlbinlog](/clients-utilities/mysqlbinlog/using-mysqlbinlog/) — Viewing the binary log with mysqlbinlog.
- [mysqlbinlog Options](/clients-utilities/mysqlbinlog/mysqlbinlog-options/) — Options supported by mysqlbinlog.
- [Annotate_rows_log_event](/clients-utilities/mysqlbinlog/annotate_rows_log_event/) — Annotate_rows events accompany row events and describe the query which caused the row event.
- [mariadb-binlog](/clients-utilities/mysqlbinlog/mariadb-binlog/) — Symlink or new name for mysqlbinlog.