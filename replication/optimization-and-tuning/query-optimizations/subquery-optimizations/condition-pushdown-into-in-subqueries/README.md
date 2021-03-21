# Condition Pushdown Into IN subqueries

This article describes Condition Pushdown into IN subqueries as implemented in [MDEV-12387](https://jira.mariadb.org/browse/MDEV-12387).

[optimizer_switch](/replication/optimization-and-tuning/query-optimizations/optimizer-switch) flag name: `condition_pushdown_for_subquery`.