# Information Schema INNODB_LOCKS Table

The [Information Schema](/kb/en/information_schema/) `INNODB_LOCKS` table stores information about locks that InnoDB transactions have requested but not yet acquired, or that are blocking another transaction.

It has the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>LOCK_ID</code></td><td>Lock ID number - the format is not fixed, so do not rely upon the number for information.</td></tr>
<tr><td><code>LOCK_TRX_ID</code></td><td>Lock's transaction ID. Matches the <a href="/kb/en/information-schema-innodb_trx-table/">INNODB_TRX.TRX_ID</a> column.</td></tr>
<tr><td><code>LOCK_MODE</code></td><td><a href="/kb/en/xtradbinnodb-lock-modes/">Lock mode</a>. One of <code>S</code> (shared), <code>X</code> (exclusive), <code>IS</code> (intention shared), <code>IX</code> (intention exclusive row lock), <code>S_GAP</code> (shared gap lock), <code>X_GAP</code> (exclusive gap lock), <code>IS_GAP</code> (intention shared gap lock), <code>IX_GAP</code> (intention exclusive gap lock) or <code>AUTO_INC</code> (<a href="/kb/en/auto_increment-handling-in-xtradbinnodb/">auto-increment table level lock</a>).</td></tr>
<tr><td><code>LOCK_TYPE</code></td><td>Whether the lock is <code>RECORD</code> (row level) or <code>TABLE</code> level.</td></tr>
<tr><td><code>LOCK_TABLE</code></td><td>Name of the locked table,or table containing locked rows.</td></tr>
<tr><td><code>LOCK_INDEX</code></td><td>Index name if a <code>RECORD</code> <code>LOCK_TYPE</code>, or <code>NULL</code> if not.</td></tr>
<tr><td><code>LOCK_SPACE</code></td><td>Tablespace ID if a <code>RECORD</code> <code>LOCK_TYPE</code>, or <code>NULL</code> if not.</td></tr>
<tr><td><code>LOCK_PAGE</code></td><td>Locked record page number if a <code>RECORD</code> <code>LOCK_TYPE</code>, or <code>NULL</code> if not.</td></tr>
<tr><td><code>LOCK_REC</code></td><td>Locked record heap number if a <code>RECORD</code> <code>LOCK_TYPE</code>, or <code>NULL</code> if not.</td></tr>
<tr><td><code>LOCK_DATA</code></td><td>Locked record primary key as an SQL string if a <code>RECORD</code> <code>LOCK_TYPE</code>, or <code>NULL</code> if not. If no primary key exists, the internal InnoDB row_id number is instead used. To avoid unnecessary IO, also <code>NULL</code> if the locked record page is not in the <a href="/kb/en/xtradbinnodb-buffer-pool/">buffer pool</a></td></tr>
</tbody></table>

The table is often used in conjunction with the [INNODB_LOCK_WAITS](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-innodb-tables/information-schema-innodb_lock_waits-table/) and [INNODB_TRX](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-innodb-tables/information-schema-innodb_trx-table/) tables to diagnose problematic locks and transactions

## Example

```sql
-- session 1
START TRANSACTION;
UPDATE t SET id = 15 WHERE id = 10;

-- session 2
DELETE FROM t WHERE id = 10;

-- session 1
USE information_schema;
SELECT l.*, t.*
    FROM information_schema.INNODB_LOCKS l
    JOIN information_schema.INNODB_TRX t
        ON l.lock_trx_id = t.trx_id
    WHERE trx_state = 'LOCK WAIT' \G
*************************** 1. row ***************************
                   lock_id: 840:40:3:2
               lock_trx_id: 840
                 lock_mode: X
                 lock_type: RECORD
                lock_table: `test`.`t`
                lock_index: PRIMARY
                lock_space: 40
                 lock_page: 3
                  lock_rec: 2
                 lock_data: 10
                    trx_id: 840
                 trx_state: LOCK WAIT
               trx_started: 2019-12-23 18:43:46
     trx_requested_lock_id: 840:40:3:2
          trx_wait_started: 2019-12-23 18:43:46
                trx_weight: 2
       trx_mysql_thread_id: 46
                 trx_query: DELETE FROM t WHERE id = 10
       trx_operation_state: starting index read
         trx_tables_in_use: 1
         trx_tables_locked: 1
          trx_lock_structs: 2
     trx_lock_memory_bytes: 1136
           trx_rows_locked: 1
         trx_rows_modified: 0
   trx_concurrency_tickets: 0
       trx_isolation_level: REPEATABLE READ
         trx_unique_checks: 1
    trx_foreign_key_checks: 1
trx_last_foreign_key_error: NULL
          trx_is_read_only: 0
trx_autocommit_non_locking: 0
```

.