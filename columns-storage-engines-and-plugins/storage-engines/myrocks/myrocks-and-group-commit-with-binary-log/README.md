# MyRocks and Group Commit with Binary log

MyRocks supports group commit with the [binary log](/mariadb-administration/server-monitoring-logs/binary-log)  ([MDEV-11934](https://jira.mariadb.org/browse/MDEV-11934)).

## Counter Values to Watch

(The following is only necessary if you are studying MyRocks internals)

MariaDB's group commit counters are:

[Binlog_commits](/kb/en/replication-and-binary-log-status-variables/#binlog_commits)  - how many transactions were written to the binary log

[Binlog_group_commits](/kb/en/replication-and-binary-log-status-variables/#binlog_group_commits) - how many group commits happened.  (e.g. if each group had two transactions, this will be twice as small as [Binlog_commits](/kb/en/replication-and-binary-log-status-variables/#binlog_commits))

On the RocksDB side, there is one relevant counter:
[Rocksdb_wal_synced](/kb/en/myrocks-status-variables/#rocksdb_wal_synced) - How many times RocksDB's WAL file was synced.  (TODO: this is after group commit happened, right?)

## On the Value of rocksdb_wal_group_syncs

FB/MySQL-5.6 has a [rocksdb_wal_group_syncs](/kb/en/myrocks-status-variables/#rocksdb_wal_group_syncs) counter (The counter is provided by MyRocks, it is not a view of a RocksDB counter). It is increased in rocksdb_flush_wal() when doing the rdb-&gt;FlushWAL() call.

rocksdb_flush_wal() is called by MySQL's Group Commit when it wants to make the effect of several rocksdb_prepare() calls persistent.

So, the value of [rocksdb_wal_group_syncs](/kb/en/myrocks-status-variables/#rocksdb_wal_group_syncs) in FB/MySQL-5.6 is similar to [Binlog_group_commits](/kb/en/replication-and-binary-log-status-variables/#binlog_group_commits) in MariaDB.

MariaDB doesn't have that call, each rocksdb_prepare() call takes care of being persistent on its own.

Because of that, [rocksdb_wal_group_syncs](/kb/en/myrocks-status-variables/#rocksdb_wal_group_syncs) is zero for MariaDB. (Currently, it is only incremented when the binlog is rotated).

## Examples

So for a workload with concurrency=50, n_queries=10K, one gets

- Binlog_commits=10K
- Binlog_group_commits=794
- Rocksdb_wal_synced=8362

This is on a RAM disk

For a workload with concurrency=50, n_queries=10K, rotating laptop hdd, one gets

- Binlog_commits= 10K
- Binlog_group_commits=1403
- Rocksdb_wal_synced=400

The test took 38 seconds, Number of syncs was 1400+400=1800, which gives 45 syncs/sec which looks normal for this slow rotating desktop hdd.

Note that the WAL was synced fewer times than there were binlog commit groups (?)