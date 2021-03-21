# Performance Schema events_transactions_history Table

##### MariaDB starting with [10.5.2](/kb/en/mariadb-1052-release-notes/)

The events_transactions_history table was introduced in [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/).

The `events_transactions_history` table contains the most recent completed transaction events for each thread.

The number of records stored per thread in the table is determined by the [performance_schema_events_transactions_history_size](/kb/en/performance-schema-system-variables/#performance_schema_events_transactions_history_size) system variable, which is autosized on startup.

If adding a completed transaction event would cause the table to exceed this limit, the oldest thread row is discarded.

All of a thread's rows are discarded when the thread ends.

The table contains the following columns:

```sql
+---------------------------------+------------------------------------------------+------+-----+---------+-------+
| Field                           | Type                                           | Null | Key | Default | Extra |
+---------------------------------+------------------------------------------------+------+-----+---------+-------+
| THREAD_ID                       | bigint(20) unsigned                            | NO   |     | NULL    |       |
| EVENT_ID                        | bigint(20) unsigned                            | NO   |     | NULL    |       |
| END_EVENT_ID                    | bigint(20) unsigned                            | YES  |     | NULL    |       |
| EVENT_NAME                      | varchar(128)                                   | NO   |     | NULL    |       |
| STATE                           | enum('ACTIVE','COMMITTED','ROLLED BACK')       | YES  |     | NULL    |       |
| TRX_ID                          | bigint(20) unsigned                            | YES  |     | NULL    |       |
| GTID                            | varchar(64)                                    | YES  |     | NULL    |       |
| XID_FORMAT_ID                   | int(11)                                        | YES  |     | NULL    |       |
| XID_GTRID                       | varchar(130)                                   | YES  |     | NULL    |       |
| XID_BQUAL                       | varchar(130)                                   | YES  |     | NULL    |       |
| XA_STATE                        | varchar(64)                                    | YES  |     | NULL    |       |
| SOURCE                          | varchar(64)                                    | YES  |     | NULL    |       |
| TIMER_START                     | bigint(20) unsigned                            | YES  |     | NULL    |       |
| TIMER_END                       | bigint(20) unsigned                            | YES  |     | NULL    |       |
| TIMER_WAIT                      | bigint(20) unsigned                            | YES  |     | NULL    |       |
| ACCESS_MODE                     | enum('READ ONLY','READ WRITE')                 | YES  |     | NULL    |       |
| ISOLATION_LEVEL                 | varchar(64)                                    | YES  |     | NULL    |       |
| AUTOCOMMIT                      | enum('YES','NO')                               | NO   |     | NULL    |       |
| NUMBER_OF_SAVEPOINTS            | bigint(20) unsigned                            | YES  |     | NULL    |       |
| NUMBER_OF_ROLLBACK_TO_SAVEPOINT | bigint(20) unsigned                            | YES  |     | NULL    |       |
| NUMBER_OF_RELEASE_SAVEPOINT     | bigint(20) unsigned                            | YES  |     | NULL    |       |
| OBJECT_INSTANCE_BEGIN           | bigint(20) unsigned                            | YES  |     | NULL    |       |
| NESTING_EVENT_ID                | bigint(20) unsigned                            | YES  |     | NULL    |       |
| NESTING_EVENT_TYPE              | enum('TRANSACTION','STATEMENT','STAGE','WAIT') | YES  |     | NULL    |       |
+---------------------------------+------------------------------------------------+------+-----+---------+-------+
```