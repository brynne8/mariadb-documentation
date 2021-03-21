# Overview of the Binary Log

The binary log contains a record of all changes to the databases, both data and structure, as well as how long each statement took to execute. It consists of a set of binary log files and an index.

This means that statements such as [CREATE](/sql-statements-structure/sql-statements/data-definition/create), [ALTER](/sql-statements-structure/sql-statements/data-definition/alter), [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert), [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update) and [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete) will be logged, but statements that have no effect on the data, such as SELECT and SHOW, will not be logged. If you want to log these (at a cost in performance), use the [general query log](/mariadb-administration/server-monitoring-logs/general-query-log).

If a statement may potentially have an effect, but doesn't, such as an UPDATE or DELETE that returns no rows, it will still be logged (this applies to the default statement-based logging, not to row-based logging - see [Binary Log Formats](/mariadb-administration/server-monitoring-logs/binary-log/binary-log-formats)).

The purpose of the binary log is to allow [replication](/replication), where data is sent from one or more masters to one or more slave servers based on the contents of the binary log, as well as assisting in backup operations.

A MariaDB server with the binary log enabled will run slightly more slowly.

It is important to protect the binary log, as it may contain sensitive information, including passwords.

Binary logs are stored in a binary, not plain text, format, and so are not viewable with a regular editor. However, MariaDB includes [mysqlbinlog](/clients-utilities/mysqlbinlog), a commandline tool for plain text processing of binary logs.