# STDDEV_SAMP

## Syntax

```sql
STDDEV_SAMP(expr)
```

## Description

Returns the sample standard deviation of `expr` (the square root of [VAR_SAMP()](/built-in-functions/aggregate-functions/var_samp/)).

It is an [aggregate function](/built-in-functions/aggregate-functions/), and so can be used with the [GROUP BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/group-by/) clause.

From [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/), STDDEV_SAMP() can be used as a [window function](/built-in-functions/special-functions/window-functions/).

STDDEV_SAMP() returns `NULL` if there were no matching rows.