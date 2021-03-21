# Improvements to ORDER BY Optimization

##### MariaDB starting with [10.1](/kb/en/what-is-mariadb-101/)

[MariaDB 10.1](/kb/en/what-is-mariadb-101/) includes several improvements to the [ORDER BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/order-by/) optimizer.

The fixes were made as a response to complaints by MariaDB customers, so they fix real-world optimization problems.  The fixes are a bit hard to describe (as the `ORDER BY` optimizer is complicated), but here's a short description:

The [ORDER BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/order-by/) optimizer in [MariaDB 10.1](/kb/en/what-is-mariadb-101/):

- Doesn’t make stupid choices when several multi-part keys and potential range accesses are present ([MDEV-6402](https://jira.mariadb.org/browse/MDEV-6402)).
<ul start="1"><li>This also fixes [MySQL Bug#12113](http://bugs.mysql.com/bug.php?id=12113).
</li></ul>
- Always uses “range” and (not full “index” scan) when it switches to an index to satisfy `ORDER BY … LIMIT` ([MDEV-6657](https://jira.mariadb.org/browse/MDEV-6657)).
- Tries hard to be smart and use cost/number of records estimates from other parts of the optimizer ([MDEV-6384](https://jira.mariadb.org/browse/MDEV-6384), [MDEV-465](https://jira.mariadb.org/browse/MDEV-465)).
<ul start="1"><li>This change also fixes [MySQL Bug#36817](http://bugs.mysql.com/bug.php?id=36817).
</li></ul>
- Takes full advantage of InnoDB’s Extended Keys feature when checking if filesort() can be skipped ([MDEV-6796](https://jira.mariadb.org/browse/MDEV-6796)).

## Extra optimizations

Starting from [MariaDB 10.1.15](/kb/en/mariadb-10115-release-notes/)

- The [ORDER BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/order-by/) optimizer takes multiple-equalities into account ([MDEV-8989](https://jira.mariadb.org/browse/MDEV-8989)). This optimization is not enabled by default in [MariaDB 10.1](/kb/en/what-is-mariadb-101/). You need to explicitly switch it ON by setting the [optimizer_switch](/replication/optimization-and-tuning/query-optimizations/optimizer-switch/) system variable, as follows:

```sql
optimizer_switch='orderby_uses_equalities=on'
```

Setting the switch ON is considered safe. It is off by default in [MariaDB 10.1](/kb/en/what-is-mariadb-101/) in order to avoid changing query plans in a stable release. It is on by default from [MariaDB 10.2](/kb/en/what-is-mariadb-102/)

## Comparison with MySQL 5.7

In [MySQL 5.7 changelog](http://mysqlserverteam.com/whats-new-in-mysql-5-7-generally-available/), one can find this passage:

Make switching of index due to small limit cost-based ([WL#6986](http://askmonty.org/worklog/?tid=6986)) : We have made
 the decision in make_join_select() of whether to switch to a new index in order to
 support "ORDER BY ... LIMIT N" cost-based. This work fixes Bug#73837.

MariaDB is not using Oracle's fix (we believe `make_join_select` is not the right place to do ORDER BY optimization),  but the effect is the same: this case is covered by [MariaDB 10.1](/kb/en/what-is-mariadb-101/)'s optimizer.

## See Also

- Blog post [MariaDB 10.1: Better query optimization for ORDER BY … LIMIT](http://s.petrunia.net/blog/?p=103)