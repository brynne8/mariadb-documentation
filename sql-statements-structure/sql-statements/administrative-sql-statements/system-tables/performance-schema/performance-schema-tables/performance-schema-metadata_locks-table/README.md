# Performance Schema metadata_locks Table

##### MariaDB starting with [10.5.2](/kb/en/mariadb-1052-release-notes/)

The metadata_locks table was introduced in [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/).

The `metadata_locks` table contains [metadata lock](/sql-statements-structure/sql-statements/transactions/metadata-locking/) information.

To enable metadata lock instrumention, at runtime:

```sql
UPDATE performance_schema.setup_instruments SET enabled='YES', timed='YES' 
  WHERE name LIKE 'wait/lock/metadata%';
```

or in the [configuration file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/):

```sql
performance-schema-instrument='wait/lock/metadata/sql/mdl=ON'
```

The table is by default autosized, but the size can be configured with the [performance_schema_max_metadata_locks](/kb/en/performance-schema-system-variables/#performance_schema_max_metadata_locks) system variabe.

The table is read-only, and [TRUNCATE TABLE](/sql-statements-structure/sql-statements/table-statements/truncate-table/) cannot be used to empty the table.

The table contains the following columns:

<table><tbody><tr><th>Field</th><th>Type</th><th>Null</th><th>Default</th><th>Description</th></tr>
<tr><td>OBJECT_TYPE</td><td>varchar(64)</td><td>NO</td><td>NULL</td><td>Object type. One of  <code>BACKUP</code>, <code>COMMIT</code>, <code>EVENT</code>, <code>FUNCTION</code>, <code>GLOBAL</code>, <code>LOCKING SERVICE</code>, <code>PROCEDURE</code>, <code>SCHEMA</code>, <code>TABLE</code>, <code>TABLESPACE</code>, <code>TRIGGER</code> (unused) or <code>USER LEVEL LOCK</code>.</td></tr>
<tr><td>OBJECT_SCHEMA</td><td>varchar(64)</td><td>YES</td><td>NULL</td><td>Object schema.</td></tr>
<tr><td>OBJECT_NAME</td><td>varchar(64)</td><td>YES</td><td>NULL</td><td>Object name.</td></tr>
<tr><td>OBJECT_INSTANCE_BEGIN</td><td>bigint(20) unsigned</td><td>NO</td><td>NULL</td><td>Address in memory of the instrumented object.</td></tr>
<tr><td>LOCK_TYPE</td><td>varchar(32)</td><td>NO</td><td>NULL</td><td>Lock type. One of <code>BACKUP_FTWRL1</code>, <code>BACKUP_START</code>, <code>BACKUP_TRANS_DML</code>, <code>EXCLUSIVE</code>, <code>INTENTION_EXCLUSIVE</code>, <code>SHARED</code>, <code>SHARED_HIGH_PRIO</code>, <code>SHARED_NO_READ_WRITE</code>, <code>SHARED_NO_WRITE</code>, <code>SHARED_READ</code>, <code>SHARED_UPGRADABLE</code> or <code>SHARED_WRITE</code>.</td></tr>
<tr><td>LOCK_DURATION</td><td>varchar(32)</td><td>NO</td><td>NULL</td><td>Lock duration. One of <code>EXPLICIT</code> (locks released by explicit action, for example a global lock acquired with <a href="/kb/en/flush/">FLUSH TABLES WITH READ LOCK</a>) , <code>STATEMENT</code> (locks implicitly released at statement end) or <code>TRANSACTION</code>  (locks implicitly released at transaction end).</td></tr>
<tr><td>LOCK_STATUS</td><td>varchar(32)</td><td>NO</td><td>NULL</td><td>Lock status. One of <code>GRANTED</code>, <code>KILLED</code>, <code>PENDING</code>, <code>POST_RELEASE_NOTIFY</code>, <code>PRE_ACQUIRE_NOTIFY</code>, <code>TIMEOUT</code> or <code>VICTIM</code>.</td></tr>
<tr><td>SOURCE</td><td>varchar(64)</td><td>YES</td><td>NULL</td><td>Source file containing the instrumented code that produced the event, as well as the line number where the instrumentation occurred. This allows one to examine the source code involved.</td></tr>
<tr><td>OWNER_THREAD_ID</td><td>bigint(20) unsigned</td><td>YES</td><td>NULL</td><td>Thread that requested the lock.</td></tr>
<tr><td>OWNER_EVENT_ID</td><td>bigint(20) unsigned</td><td>YES</td><td>NULL</td><td>Event that requested the lock.</td></tr>
</tbody></table>