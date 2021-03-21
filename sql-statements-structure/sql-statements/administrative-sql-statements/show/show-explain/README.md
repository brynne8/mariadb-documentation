# SHOW EXPLAIN

##### MariaDB starting with [10.0.0](/kb/en/mariadb-1000-release-notes/)

The `SHOW EXPLAIN` command was added in [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/).

## Syntax

```sql
SHOW EXPLAIN FOR <thread_id>;
```

## Description

The `SHOW EXPLAIN` command allows one to get an [EXPLAIN](/sql-statements-structure/sql-statements/administrative-sql-statements/analyze-and-explain-statements/explain) (that is, a
description of a query plan) of a query running in a certain thread.

```sql
SHOW EXPLAIN FOR <thread_id>;
```

will produce an `EXPLAIN` output for the query that thread number `thread_id` is running. The thread id can be obtained with [SHOW PROCESSLIST](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-processlist).

```sql
SHOW EXPLAIN FOR 1;
+------+-------------+-------+-------+---------------+------+---------+------+---------+-------------+
| id   | select_type | table | type  | possible_keys | key  | key_len | ref  | rows    | Extra       |
+------+-------------+-------+-------+---------------+------+---------+------+---------+-------------+
|    1 | SIMPLE      | tbl   | index | NULL          | a    | 5       | NULL | 1000107 | Using index |
+------+-------------+-------+-------+---------------+------+---------+------+---------+-------------+
1 row in set, 1 warning (0.00 sec)
```

The output is always accompanied with a warning which shows the query the
target thread is running (this shows what the `EXPLAIN` is for):

```sql
SHOW WARNINGS;
+-------+------+------------------------+
| Level | Code | Message                |
+-------+------+------------------------+
| Note  | 1003 | select sum(a) from tbl |
+-------+------+------------------------+
1 row in set (0.00 sec)
```

### Possible Errors

The output can be only produced if the target thread is <em>currently</em> running a
query, which has a ready query plan. If this is not the case, the output will
be:

```sql
SHOW EXPLAIN FOR 2;
ERROR 1932 (HY000): Target is not running an EXPLAINable command
```

You will get this error when:

- the target thread is not running a command for which one can run `EXPLAIN`
- the target thread is running a command for which one can run `EXPLAIN`, but
<ul start="1"><li>there is no query plan yet (for example, tables are open and locks are
   acquired before the query plan is produced)
</li></ul>

### Differences Between SHOW EXPLAIN and EXPLAIN Outputs

#### Background

In MySQL, `EXPLAIN` execution takes a slightly different route from the way
the real query (typically the `SELECT`) is optimized. This is unfortunate,
and has caused a number of bugs in `EXPLAIN`. (For example, see
[MDEV-326](https://jira.mariadb.org/browse/MDEV-326), [MDEV-410](https://jira.mariadb.org/browse/MDEV-410), and
[lp:1013343](https://bugs.launchpad.net/maria/+bug/1013343).
[lp:992942](https://bugs.launchpad.net/maria/+bug/992942) is not directly
about `EXPLAIN`, but it also would not have existed if MySQL didn't try to delete
parts of a query plan in the middle of the query)

`SHOW EXPLAIN` examines a running `SELECT`, and hence its output may be
slightly different from what `EXPLAIN SELECT` would produce. We did our best
to make sure that either the difference is negligible, or `SHOW EXPLAIN`'s
output is closer to reality than `EXPLAIN`'s output.

#### List of Recorded Differences

- `SHOW EXPLAIN` may have Extra='`no matching row in const table`', where  `EXPLAIN` would produce Extra='`Impossible WHERE ...`'
- For queries with subqueries, `SHOW EXPLAIN` may print `select_type==PRIMARY` where regular `EXPLAIN` used to print `select_type==SIMPLE`, or vice versa.

#### Required Permissions

Running `SHOW EXPLAIN` requires the same permissions as
running `SHOW PROCESSLIST` would.

## See Also

- [EXPLAIN ANALYZE](/sql-statements-structure/sql-statements/administrative-sql-statements/analyze-and-explain-statements/explain-analyze), which will perform a query and outputs enhanced `EXPLAIN` results.
- It is also possible to [save EXPLAIN into the slow query log](/mariadb-administration/server-monitoring-logs/slow-query-log/explain-in-the-slow-query-log).