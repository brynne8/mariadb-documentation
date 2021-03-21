# Enhancements for START TRANSACTION WITH CONSISTENT SNAPSHOT

With the introduction of [group commit](/mariadb-administration/server-monitoring-logs/binary-log/group-commit-for-the-binary-log/), MariaDB also introduced an enhanced storage engine API for COMMIT that allows engines to coordinate commit ordering and visibility with each other and with the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/).

With these improvements, the `START TRANSACTION WITH CONSISTENT SNAPSHOT` statement was enhanced to ensure consistency between storage engines that support the new
API. At the time of writing, the supporting engines are [XtraDB](/kb/en/xtradb-and-innodb/) and [PBXT](/columns-storage-engines-and-plugins/storage-engines/legacy-storage-engines/pbxt-storage-engine/). In
addition, the binary log, while not a storage engine as such, also supports
the new API and can provide a binlog position consistent with storage engine
transaction snapshots.

This means that with transaction isolation level at least [REPEATABLE READ](/kb/en/set-transaction/#repeatable-read), the
`START TRANSACTION WITH CONSISTENT SNAPSHOT` statement can be used to ensure
that queries will see a transaction-consistent view of the database also
between storage engines. It is then not possible for a query to see the
changes from some transaction T in XtraDB tables without also seeing the
changes T makes to PBXT tables. (Within a single transactional storage engine
like XtraDB or PBXT, consistency is always guaranteed even without using `START TRANSACTION WITH CONSISTENT SNAPSHOT`).

For example, suppose the following two transactions run in parallel:

Transaction T1:

```sql
    BEGIN;
    SET @t = NOW();
    UPDATE xtradb_table SET a= @t WHERE id = 5;
    UPDATE pbxt_table SET b= @t WHERE id = 5;
    COMMIT;
```

Transaction T2:

```sql
    SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;
    START TRANSACTION WITH CONSISTENT SNAPSHOT;
    SELECT t1.a, t2.b
      FROM xtradb_table t1 INNER JOIN pbxt_table t2 ON t1.id=t2.id
     WHERE t1.id = 5;
```

Then transaction T2 will always see the same value for `xtradb_table.a` and
`pbxt_table.b`.

(In [MariaDB 5.2](/kb/en/what-is-mariadb-52/) and earlier, and MySQL at least up to 5.5, <code>START TRANSACTION
WITH CONSISTENT SNAPSHOT</code> did not give any guarantees of consistency between
different storage engines. So it is possible, even with a "consistent"
snapshot, to see the changes in a transaction only to InnoDB/XtraDB tables, not
PBXT tables, for example.)

## Status Variables

Another use for these enhancements is to obtain
a binary log position that is consistent
with a particular transactional state of the storage engine(s) in the
database. This is done with two status variables for the binary log: [binlog_snapshot_file](/kb/en/replication-and-binary-log-status-variables/#binlog_snapshot_file) and [binlog_snapshot_position](/kb/en/replication-and-binary-log-status-variables/#binlog_snapshot_position)

These variables give the binary log file and position like `SHOW MASTER STATUS`
does. But they can be queried in a transactionally consistent way. After
starting a transaction using `START TRANSACTION WITH CONSISTENT SNAPSHOT`, the
two variables will give the binlog position corresponding to the state of the
database of the consistent snapshot so taken, irrespectively of which other
transactions have been committed since the snapshot was taken (<code>SHOW MASTER
STATUS</code> always shows the position of the last committed transaction). This works for
MVCC storage engines that implement the commit ordering part of the storage
engine API, which at the time of writing is XtraDB and PBXT.

This is useful to obtain a logical dump/backup with a matching binlog position
that can be used to provision a new slave with the original server as the
master. First `START TRANSACTION WITH CONSISTENT SNAPSHOT` is executed. Then a
consistent state of the database is obtained with queries, and the matching
binlog position is obtained with `SHOW STATUS LIKE 'binlog_snapshot%'`. When
this is loaded on a new slave server, the binlog position is used in a <code>CHANGE
MASTER TO</code> statement to set the slave replicating from the correct position.

With the variables binlog_snapshot_file and binlog_snapshot_position, such
provisioning can be done fully non-blocking on the master. Without them, it is
necessary to get the binlog position under `FLUSH TABLES WITH READ LOCK`;
this can potentially stall the server for a long time, as it blocks new
queries until all updates that have already started have finished.

### mysqldump

The [mysqldump](/clients-utilities/backup-restore-and-import-clients/mysqldump/) program was extended to use these status
variables. This means that a backup suitable for provisioning a slave can be
obtained as normal like this:

```sql
    mysqldump --single-transaction --master-data ...
```

The dump will be fully non-blocking if both the mysqldump program and the
queried server include the necessary feature (eg. both are from [MariaDB
5.2](/kb/en/what-is-mariadb-52/)-rpl, 5.3, or higher). In other cases, it will fall back to the old
blocking method using `FLUSH TABLES WITH READ LOCK`.

For more information on the design and implementation of this feature, see [MWL#136](http://askmonty.org/worklog/?tid=136).

## See Also

- [START TRANSACTION](/sql-statements-structure/sql-statements/transactions/start-transaction/)
- [What is MariaDB 5.3](/kb/en/what-is-mariadb-53/)
- [MyRocks and START TRANSACTION WITH CONSISTENT SNAPSHOT](/columns-storage-engines-and-plugins/storage-engines/myrocks/myrocks-and-start-transaction-with-consistent-snapshot/)