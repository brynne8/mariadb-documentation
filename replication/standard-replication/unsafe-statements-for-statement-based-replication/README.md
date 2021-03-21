# Unsafe Statements for Statement-based Replication

The terms <em>master</em> and <em>slave</em> have historically been used in replication, but the terms terms <em>primary</em> and <em>replica</em> are now preferred. The old terms are used throughout the documentation, and in MariaDB commands, although [MariaDB 10.5](/kb/en/what-is-mariadb-105/) has begun the process of renaming. The documentation will follow over time. See [MDEV-18777](https://jira.mariadb.org/browse/MDEV-18777) to follow progress on this effort.

A safe statement is generally deterministic; in other words the statement will always produce the same result. For example, an INSERT statement producing a random number will most likely produce a different result on the master than on the slave, and so cannot be replicated safely.

When an unsafe statement is run, the current binary logging format determines how the server responds.

- If the binary logging format is [statement-based](/kb/en/binary-log-formats/#statement-based-logging) (the default until [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/)), unsafe statements generate a warning and are logged normally.
- If the binary logging format is [mixed](/kb/en/binary-log-formats/#mixed-logging) (the default from [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/)), unsafe statements are logged using the row-based format, while safe statements use the statement-based format.
- If the binary logging format is [row-based](/kb/en/binary-log-formats/#row-based-logging), all statements are logged normally, and the distinction between safe and unsafe is not made.

MariaDB tries to detect unsafe statements. When an unsafe statement is issued, a warning similar to the following is produced:

```sql
Note (Code 1592): Unsafe statement written to the binary log using statement format since 
  BINLOG_FORMAT = STATEMENT. The statement is unsafe because it uses a LIMIT clause. This 
  is unsafe because the set of rows included cannot be predicted.
```

MariaDB also issues this warning for some classes of statements that are safe.

## Unsafe Statements

The following statements are regarded as unsafe:

- [INSERT ... ON DUPLICATE KEY UPDATE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-on-duplicate-key-update/) statements upon tables with multiple primary or unique keys, as the order that the keys are checked in, and which affect the rows chosen to update, is not deterministic. Before [MariaDB 5.5.24](/kb/en/mariadb-5524-release-notes/), these statements were not regarded as unsafe. In [MariaDB 10.0](/kb/en/what-is-mariadb-100/) this warning has been removed as we always check keys in the same order on the master and slave if the master and slave are using the same storage engine.
- [INSERT-DELAYED](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-delayed/).  These statements are inserted in an indeterminate order.
- [INSERT's](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert/) on tables with a composite primary key that has an [AUTO_INCREMENT](/columns-storage-engines-and-plugins/data-types/auto_increment/) column that isn't the first column of the composite key.
- When a table has an [AUTO_INCREMENT](/columns-storage-engines-and-plugins/data-types/auto_increment/) column and a [trigger](/programming-customizing-mariadb/triggers-events/triggers/) or [stored procedure](/kb/en/stored-programs-and-views/) executes an [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update/) statement against the table. Before [MariaDB 5.5](/kb/en/what-is-mariadb-55/), all updates on tables with an AUTO_INCREMENT column were considered unsafe, as the order that the rows were updated could differ across servers.
- [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update/) statements that use [LIMIT](/kb/en/select/#limit), since the order of the returned rows is unspecified. This applies even to statements using an ORDER BY clause, which are deterministic (a known bug). However, since [MariaDB 10.0.11](/kb/en/mariadb-10011-release-notes/), `LIMIT 0` is an exception to this rule (see [MDEV-6170](https://jira.mariadb.org/browse/MDEV-6170)), and these statements are safe for replication.
- When using a [user-defined function](/programming-customizing-mariadb/user-defined-functions/).
- Statements using using any of the following functions, which can return different results on the slave:
<ul start="1"><li>[FOUND_ROWS()](/built-in-functions/secondary-functions/information-functions/found_rows/)
</li><li>[GET_LOCK()](/built-in-functions/secondary-functions/miscellaneous-functions/get_lock/)
</li><li>[IS_FREE_LOCK()](/built-in-functions/secondary-functions/miscellaneous-functions/is_free_lock/)
</li><li>[IS_USED_LOCK()](/built-in-functions/secondary-functions/miscellaneous-functions/is_used_lock/)
</li><li>[LOAD_FILE()](/built-in-functions/string-functions/load_file/)
</li><li>[MASTER_POS_WAIT()](/built-in-functions/secondary-functions/miscellaneous-functions/master_pos_wait/)
</li><li>[RAND()](/built-in-functions/numeric-functions/rand/)
</li><li>[RELEASE_ALL_LOCKS()](/built-in-functions/secondary-functions/miscellaneous-functions/release_all_locks/)
</li><li>[RELEASE_LOCK()](/built-in-functions/secondary-functions/miscellaneous-functions/release_lock/)
</li><li>[ROW_COUNT()](/built-in-functions/secondary-functions/information-functions/row_count/)
</li><li>[SESSION_USER()](/built-in-functions/secondary-functions/information-functions/session_user/)
</li><li>[SLEEP()](/built-in-functions/secondary-functions/miscellaneous-functions/sleep/), [SYSDATE()](/built-in-functions/date-time-functions/sysdate/)
</li><li>[SYSTEM_USER()](/built-in-functions/secondary-functions/information-functions/system_user/)
</li><li>[USER()](/built-in-functions/secondary-functions/information-functions/user/)
</li><li>[UUID()](/built-in-functions/secondary-functions/miscellaneous-functions/uuid/)
</li><li>[UUID_SHORT()](/built-in-functions/secondary-functions/miscellaneous-functions/uuid_short/). 
</li></ul>
- Statements which refer to log tables, since these may differ across servers.
- Statements which refer to self-logging tables. Statements following a read or write to a self-logging table within a transaction are also considered unsafe.
- Statements which refer to [system variables](/replication/optimization-and-tuning/system-variables/server-system-variables/) (there are a few exceptions).
- [LOAD DATA INFILE](/kb/en/load-data-infile/) statements (since [MariaDB 5.5](/kb/en/what-is-mariadb-55/)).
- Non-transactional reads or writes that execute after transactional reads within a transaction.
- If row-based logging is used for a statement, and the session executing the statement has any temporary tables, row-based logging is used for the remaining statements until the temporary table is dropped. This is because temporary tables can't use row-based logging, so if it is used due to one of the above conditions, all subsequent statements using that table are unsafe. The server deals with this situation by treating all statements in the session as unsafe for statement-based logging until the temporary table is dropped.

## Safe Statements

The following statements are not deterministic, but are considered safe for binary logging and replication:

- [CONNECTION_ID()](/built-in-functions/secondary-functions/information-functions/connection_id/)
- [CURDATE()](/built-in-functions/date-time-functions/curdate/)
- [CURRENT_DATE()](/built-in-functions/date-time-functions/current_date/)
- [CURRENT_TIME()](/built-in-functions/date-time-functions/current_time/)
- [CURRENT_TIMESTAMP()](/built-in-functions/date-time-functions/current_timestamp/)
- [CURTIME()](/built-in-functions/date-time-functions/curtime/)
- [LAST_INSERT_ID()](/built-in-functions/secondary-functions/information-functions/last_insert_id/)
- [LOCALTIME()](/built-in-functions/date-time-functions/localtime/)
- [LOCALTIMESTAMP()](/built-in-functions/date-time-functions/localtimestamp/)
- [NOW()](/built-in-functions/date-time-functions/now/)
- [UNIX_TIMESTAMP()](/built-in-functions/date-time-functions/unix_timestamp/)
- [UTC_DATE()](/built-in-functions/date-time-functions/utc_date/)
- [UTC_TIME()](/built-in-functions/date-time-functions/utc_time/)
- [UTC_TIMESTAMP()](/built-in-functions/date-time-functions/utc_timestamp/)

## Isolation Levels

Even when using safe statements, not all [transaction isolation levels](/kb/en/set-transaction/#isolation-levels) are safe with statement-based or mixed binary logging. The REPEATABLE READ and SERIALIZABLE isolation levels can only be used with the row-based format.

This restriction does not apply if only non-transactional storage engines are used.

## See Also

- [Replication and Foreign Keys](/replication/standard-replication/replication-and-foreign-keys/)