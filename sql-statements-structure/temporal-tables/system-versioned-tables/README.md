# System-Versioned Tables

MariaDB supports temporal data tables in the form of system-versioning tables (allowing you to query and operate on historic data, discussed below), [application-time periods](/sql-statements-structure/temporal-tables/application-time-periods) (allow you to query and operate on a temporal range of data), and [bitemporal tables](/sql-statements-structure/temporal-tables/bitemporal-tables) (which combine both system-versioning and [application-time periods](/sql-statements-structure/temporal-tables/application-time-periods)).

## System-Versioned Tables

<br>

##### MariaDB starting with [10.3.4](/kb/en/mariadb-1034-release-notes/)

Support for system-versioned tables was added in [MariaDB 10.3.4](/kb/en/mariadb-1034-release-notes/).

System-versioned tables store the history of all changes, not only data which is currently valid. This allows data analysis for any point in time, auditing of changes and comparison of data from different points in time.
Typical uses cases are:

- Forensic analysis &amp; legal requirements to store data for N years.
- Data analytics (retrospective, trends etc.), e.g. to get your staff information as of one year ago.
- Point-in-time recovery - recover a table state as of particular point in time.

System-versioned tables were first introduced in the SQL:2011 standard.

### Creating a System-Versioned Table

<br>
The [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table) syntax has been extended to permit creating a system-versioned table. To be system-versioned, according to SQL:2011, a table must have two generated columns, a period, and a special table option clause:
<br><br>

```sql
CREATE TABLE t(
   x INT,
   start_timestamp TIMESTAMP(6) GENERATED ALWAYS AS ROW START,
   end_timestamp TIMESTAMP(6) GENERATED ALWAYS AS ROW END,
   PERIOD FOR SYSTEM_TIME(start_timestamp, end_timestamp)
) WITH SYSTEM VERSIONING;
```

In MariaDB one can also use a simplified syntax:

```sql
CREATE TABLE t (
   x INT
) WITH SYSTEM VERSIONING;
```

In the latter case no extra columns will be created and they won't clutter the output of, say, `SELECT * FROM t`. The versioning information will still be stored, and it can be accessed via the pseudo-columns `ROW_START` and `ROW_END`:

```sql
SELECT x, ROW_START, ROW_END FROM t;
```

### Adding or Removing System Versioning To/From a Table

An existing table can be [altered](/sql-statements-structure/sql-statements/data-definition/alter/alter-table) to enable system versioning for it.

```sql
CREATE TABLE t(
  x INT
);
```

```sql
ALTER TABLE t ADD SYSTEM VERSIONING;
```

```sql
SHOW CREATE TABLE t\G
*************************** 1. row ***************************
       Table: t
Create Table: CREATE TABLE `t` (
  `x` int(11) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=latin1 WITH SYSTEM VERSIONING
```

Similarly, system versioning can be removed from a table:

```sql
ALTER TABLE t DROP SYSTEM VERSIONING;
```

```sql
SHOW CREATE TABLE t\G
*************************** 1. row ***************************
       Table: t
Create Table: CREATE TABLE `t` (
  `x` int(11) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=latin1
```

One can also add system versioning with all columns created explicitly:

```sql
ALTER TABLE t ADD COLUMN ts TIMESTAMP(6) GENERATED ALWAYS AS ROW START,
              ADD COLUMN te TIMESTAMP(6) GENERATED ALWAYS AS ROW END,
              ADD PERIOD FOR SYSTEM_TIME(ts, te),
              ADD SYSTEM VERSIONING;
```

```sql
SHOW CREATE TABLE t\G
*************************** 1. row ***************************
       Table: t
Create Table: CREATE TABLE `t` (
  `x` int(11) DEFAULT NULL,
  `ts` timestamp(6) GENERATED ALWAYS AS ROW START,
  `te` timestamp(6) GENERATED ALWAYS AS ROW END,
  PERIOD FOR SYSTEM_TIME (`ts`, `te`)
) ENGINE=InnoDB DEFAULT CHARSET=latin1 WITH SYSTEM VERSIONING
```

### Querying Historical Data

#### `SELECT`

To query the historical data one uses the clause `FOR SYSTEM_TIME` directly after the table name (before the table alias, if any). SQL:2011 provides three syntactic extensions:

- `AS OF` is used to see the table as it was at a specific point in time in the past:

```sql
SELECT * FROM t FOR SYSTEM_TIME AS OF TIMESTAMP'2016-10-09 08:07:06';
```

- `BETWEEN start AND end` will show all rows that were visible at any point between two specified points in time. It works inclusively, a row visible exactly at <em>start</em> or exactly at <em>end</em> will be shown too.

```sql
SELECT * FROM t FOR SYSTEM_TIME BETWEEN (NOW() - INTERVAL 1 YEAR) AND NOW();
```

- `FROM start TO end` will also show all rows that were visible at any point between two specified points in time, including <em>start</em>, but <strong>excluding</strong> <em>end</em>.

```sql
SELECT * FROM t FOR SYSTEM_TIME FROM '2016-01-01 00:00:00' TO '2017-01-01 00:00:00';
```

Additionally MariaDB implements a non-standard extension:

- `ALL` will show all rows, historical and current.

```sql
SELECT * FROM t FOR SYSTEM_TIME ALL;
```

If the `FOR SYSTEM_TIME` clause is not used, the table will show the <em>current</em> data, as if one had specified `FOR SYSTEM_TIME AS OF CURRENT_TIMESTAMP`.

#### Views and Subqueries

When a system-versioned tables is used in a view or in a subquery in the from clause, `FOR SYSTEM_TIME` can be used directly in the view or subquery body, or (non-standard) applied to the whole view when it's being used in a `SELECT`:

```sql
CREATE VIEW v1 AS SELECT * FROM t FOR SYSTEM_TIME AS OF TIMESTAMP'2016-10-09 08:07:06';
```

Or

```sql
CREATE VIEW v1 AS SELECT * FROM t;
SELECT * FROM v1 FOR SYSTEM_TIME AS OF TIMESTAMP'2016-10-09 08:07:06';
```

#### Use in Replication and Binary Logs

Tables that use system-versioning implicitly add the `row_end` column to the Primary Key.  While this is generally not an issue for most use cases, it can lead to problems when re-applying write statements from the binary log or in replication environments, where a master retries an SQL statement on the slave.

Specifically, these writes include a value on the `row_end` column containing the timestamp from when the write was initially made.  The re-occurrence of the Primary Key with the old system-versioning columns raises an error due to the duplication.

To mitigate this with MariaDB Replication, set the [secure_timestamp](/kb/en/server-system-variables/#secure_timestamp) system variable to `YES` on the slave. When set, the slave uses its own system clock when applying to the row log, meaning that the master can retry as many times as needed without causing a conflict.  The retries generate new historical rows with new values for the `row_start` and `row_end` columns.

### Transaction-Precise History in InnoDB

A point in time when a row was inserted or deleted does not necessarily mean that a change became visible at the same moment. With transactional tables, a row might have been inserted in a long transaction, and became visible hours after it was inserted.

For some applications — for example, when doing data analytics on one-year-old data — this distinction does not matter much. For others — forensic analysis — it might be crucial.

MariaDB supports transaction-precise history (only for the [InnoDB storage engine](/columns-storage-engines-and-plugins/storage-engines/innodb)) that allows seeing the data exactly as it would've been seen by a new connection doing a `SELECT` at the specified point in time — rows inserted <em>before</em> that point, but committed <em>after</em> will not be shown.

To use transaction-precise history, InnoDB needs to remember not timestamps, but transaction identifier per row. This is done by creating generated columns as `BIGINT UNSIGNED`, not `TIMESTAMP(6)`:

```sql
CREATE TABLE t(
   x INT,
   start_trxid BIGINT UNSIGNED GENERATED ALWAYS AS ROW START,
   end_trxid BIGINT UNSIGNED GENERATED ALWAYS AS ROW END,
   PERIOD FOR SYSTEM_TIME(start_trxid, end_trxid)
) WITH SYSTEM VERSIONING;
```

These columns must be specified explicitly, but they can be made [INVISIBLE](/sql-statements-structure/sql-statements/data-definition/create/invisible-columns) to avoid cluttering `SELECT *` output.

When one uses transaction-precise history, one can optionally use transaction identifiers in the `FOR SYSTEM_TIME` clause:

```sql
SELECT * FROM t FOR SYSTEM_TIME AS OF TRANSACTION 12345;
```

This will show the data, exactly as it was seen by the transaction with the identifier 12345.

### Storing the History Separately

When the history is stored together with the current data, it increases the size of the table, so current data queries — table scans and index searches — will take more time, because they will need to skip over historical data. If most queries on that table use only current data, it might make sense to store the history separately, to reduce the overhead from versioning.

This is done by partitioning the table by `SYSTEM_TIME`. Because of the [partition pruning](/mariadb-administration/partitioning-tables/partition-pruning-and-selection) optimization, all current data queries will only access one partition, the one that stores current data.

This example shows how to create such a partitioned table:

```sql
CREATE TABLE t (x INT) WITH SYSTEM VERSIONING
  PARTITION BY SYSTEM_TIME (
    PARTITION p_hist HISTORY,
    PARTITION p_cur CURRENT
  );
```

In this example all history will be stored in the partition `p_hist` while all current data will be in the partition `p_cur`. The table must have exactly one current partition and at least one historical partition.

Partitioning by `SYSTEM_TIME` also supports automatic partition rotation. One can rotate historical partitions by time or by size. This example shows how to rotate partitions by size:

```sql
CREATE TABLE t (x INT) WITH SYSTEM VERSIONING
  PARTITION BY SYSTEM_TIME LIMIT 100000 (
    PARTITION p0 HISTORY,
    PARTITION p1 HISTORY,
    PARTITION pcur CURRENT
  );
```

MariaDB will start writing history rows into partition `p0`, and when it reaches a size of 100000 rows, MariaDB will switch to partition `p1`. There are only two historical partitions, so when `p1` overflows, MariaDB will issue a warning, but will continue writing into it.

Similarly, one can rotate partitions by time:

```sql
CREATE TABLE t (x INT) WITH SYSTEM VERSIONING
  PARTITION BY SYSTEM_TIME INTERVAL 1 WEEK (
    PARTITION p0 HISTORY,
    PARTITION p1 HISTORY,
    PARTITION p2 HISTORY,
    PARTITION pcur CURRENT
  );
```

This means that the history for the first week after the table was created will be stored in `p0`. The history for the second week — in `p1`, and all later history will go into `p2`. One can see the exact rotation time for each partition in the [INFORMATION_SCHEMA.PARTITIONS](/kb/en/information-schema-partitions-table/) table.

It is possible to combine partitioning by `SYSTEM_TIME` and subpartitions:

```sql
CREATE TABLE t (x INT) WITH SYSTEM VERSIONING
  PARTITION BY SYSTEM_TIME
    SUBPARTITION BY KEY (x)
    SUBPARTITIONS 4 (
    PARTITION ph HISTORY,
    PARTITION pc CURRENT
  );
```

#### Default Partitions

##### MariaDB starting with [10.5.0](/kb/en/mariadb-1050-release-notes/)

Since partitioning by current and historical data is such a typical usecase, from [MariaDB 10.5](/kb/en/what-is-mariadb-105/), it is possible to use a simplified statement to do so. For example, instead of

```sql
CREATE TABLE t (x INT) WITH SYSTEM VERSIONING 
  PARTITION BY SYSTEM_TIME (
    PARTITION p0 HISTORY,  
    PARTITION pn CURRENT 
);
```

you can use

```sql
CREATE TABLE t (x INT) WITH SYSTEM VERSIONING 
  PARTITION BY SYSTEM_TIME;
```

You can also specify the number of partitions, which is useful if you want to rotate history by time, for example:

```sql
CREATE TABLE t (x INT) WITH SYSTEM VERSIONING 
  PARTITION BY SYSTEM_TIME 
    INTERVAL 1 MONTH 
    PARTITIONS 12;
```

Specifying the number of partitions without specifying a rotation condition will result in a warning:

```sql
CREATE OR REPLACE TABLE t (x INT) WITH SYSTEM VERSIONING
  PARTITION BY SYSTEM_TIME PARTITIONS 12;
Query OK, 0 rows affected, 1 warning (0.518 sec)

Warning (Code 4115): Maybe missing parameters: no rotation condition for multiple HISTORY partitions.
```

while specifying only 1 partition will result in an error:

```sql
CREATE OR REPLACE TABLE t (x INT) WITH SYSTEM VERSIONING
  PARTITION BY SYSTEM_TIME PARTITIONS 1;
ERROR 4128 (HY000): Wrong partitions for `t`: must have at least one HISTORY and exactly one last CURRENT
```

### Removing Old History

Because it stores all the history, a system-versioned table might grow very large over time. There are many options to trim down the space and remove the old history.

One can completely drop the versioning from the table and add it back again, this will delete all the history:

```sql
ALTER TABLE t DROP SYSTEM VERSIONING;
ALTER TABLE t ADD SYSTEM VERSIONING;
```

It might be a rather time-consuming operation, though, as the table will need to be rebuilt, possibly twice (depending on the storage engine).

Another option would be to use partitioning and drop some of historical partitions:

```sql
ALTER TABLE t DROP PARTITION p0;
```

Note, that one cannot drop a current partition or the only historical partition.

And the third option; one can use a variant of the [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete) statement to prune the history:

```sql
DELETE HISTORY FROM t;
```

or only old history up to a specific point in time:

```sql
DELETE HISTORY FROM t BEFORE SYSTEM_TIME '2016-10-09 08:07:06';
```

or to a specific transaction (with `BEFORE SYSTEM_TIME TRANSACTION xxx`).

To protect the integrity of the history, this statement requires a special [`DELETE HISTORY`](/kb/en/grant/#table-privileges) privilege.

The [TRUNCATE TABLE](/sql-statements-structure/sql-statements/table-statements/truncate-table) statement drops all historical records from a system-versioned-table.

### Excluding Columns From Versioning

Another MariaDB extension allows to version only a subset of columns in a table. This is useful, for example, if you have a table with user information that should be versioned, but one column is, let's say, a login counter that is incremented often and is not interesting to version. Such a column can be excluded from versioning by declaring it `WITHOUT VERSIONING`

```sql
CREATE TABLE t (
   x INT,
   y INT WITHOUT SYSTEM VERSIONING
) WITH SYSTEM VERSIONING;
```

A column can also be declared `WITH VERSIONING`, that will automatically make the table versioned. The statement below is equivalent to the one above:

```sql
CREATE TABLE t (
   x INT WITH SYSTEM VERSIONING,
   y INT
);
```

Changes in other sections:
[https://mariadb.com/kb/en/create-table/](/sql-statements-structure/sql-statements/data-definition/create/create-table)
[https://mariadb.com/kb/en/alter-table/](/sql-statements-structure/sql-statements/data-definition/alter/alter-table)
[https://mariadb.com/kb/en/join-syntax/](https://mariadb.com/kb/en/join-syntax/)
[https://mariadb.com/kb/en/partitioning-types-overview/](/mariadb-administration/partitioning-tables/partitioning-types/partitioning-types-overview)
[https://mariadb.com/kb/en/date-and-time-units/](/built-in-functions/date-time-functions/date-and-time-units)
[https://mariadb.com/kb/en/delete/](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete)
[https://mariadb.com/kb/en/library/grant/](/sql-statements-structure/sql-statements/account-management-sql-commands/grant)

they all reference back to this page

Also, TODO:

- limitations (size, speed, adding history to unique not nullable columns)

## System Variables

There are a number of system variables related to system-versioned tables:

#### `system_versioning_alter_history`

- <strong>Description:</strong> SQL:2011 does not allow [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table) on system-versioned tables. When this variable is set to `ERROR`, an attempt to alter a system-versioned table will result in an error. When this variable is set to `KEEP`, ALTER TABLE will be allowed, but the history will become incorrect — querying historical data will show the new table structure. This mode is still useful, for example, when adding new columns to a table. Note that if historical data contains or would contain nulls, attempting to ALTER these columns to be `NOT NULL` will return an error (or warning if [strict_mode](/kb/en/sql-mode/#strict-mode) is not set).
- <strong>Commandline:</strong> `--system-versioning-alter-history=value`
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Type:</strong> Enum
- <strong>Default Value:</strong> `ERROR`
- <strong>Valid Values:</strong> `ERROR`, `KEEP`
- <strong>Introduced:</strong> [MariaDB 10.3.4](/kb/en/mariadb-1034-release-notes/)

---

#### `system_versioning_asof`

- <strong>Description:</strong> If set to a specific timestamp value, an implicit `FOR SYSTEM_TIME AS OF` clause will be applied to all queries. This is useful if one wants to do many queries for history at the specific point in time. Set it to `DEFAULT` to restore the default behavior. Has no effect on DML, so queries such as [INSERT .. SELECT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-select) and [REPLACE .. SELECT](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/replace) need to state AS OF explicitly.
- <strong>Commandline:</strong> None
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Type:</strong> Varchar
- <strong>Default Value:</strong> `DEFAULT`
- <strong>Introduced:</strong> [MariaDB 10.3.4](/kb/en/mariadb-1034-release-notes/)

---

#### `system_versioning_innodb_algorithm_simple`

- <strong>Description:</strong> Never fully implemented and removed in the following release.
- <strong>Commandline:</strong> `--system-versioning-innodb-algorithm-simple[={0|1}]`
- <strong>Scope:</strong> Global, Session
- <strong>Dynamic:</strong> Yes
- <strong>Type:</strong> Boolean
- <strong>Default Value:</strong> `ON`
- <strong>Introduced:</strong> [MariaDB 10.3.4](/kb/en/mariadb-1034-release-notes/)
- <strong>Removed:</strong> [MariaDB 10.3.5](/kb/en/mariadb-1035-release-notes/)

---

## Limitations

- Versioning clauses can not be applied to [generated (virtual and persistent) columns](/sql-statements-structure/sql-statements/data-definition/create/generated-columns).
- [mysqldump](/clients-utilities/backup-restore-and-import-clients/mysqldump) does not read historical rows from versioned tables, and so historical data will not be backed up. Also, a restore of the timestamps would not be possible as they cannot be defined by an insert/a user.

## See Also

- [Application-Time Periods](/sql-statements-structure/temporal-tables/application-time-periods)
- [Bitemporal Tables](/sql-statements-structure/temporal-tables/bitemporal-tables)
- [mysql.transaction_registry Table](/kb/en/mysqltransaction_registry-table/)
- [MariaDB Temporal Tables](https://youtu.be/uBoUlTsU1Tk) (video)