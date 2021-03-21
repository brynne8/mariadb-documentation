# NOT BETWEEN

## Syntax

```sql
expr NOT BETWEEN min AND max
```

## Description

This is the same as NOT (expr [BETWEEN](/sql-statements-structure/operators/comparison-operators/between-and) min AND max).

Note that the meaning of the alternative form <code class="highlight fixed" style="white-space:pre-wrap">NOT expr BETWEEN min AND max</code> is affected by the `HIGH_NOT_PRECEDENCE` [SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode) flag.

## Examples

```sql
SELECT 1 NOT BETWEEN 2 AND 3;
+-----------------------+
| 1 NOT BETWEEN 2 AND 3 |
+-----------------------+
|                     1 |
+-----------------------+
```

```sql
SELECT 'b' NOT BETWEEN 'a' AND 'c';
+-----------------------------+
| 'b' NOT BETWEEN 'a' AND 'c' |
+-----------------------------+
|                           0 |
+-----------------------------+
```

NULL:

```sql
SELECT 1 NOT BETWEEN 1 AND NULL;
+--------------------------+
| 1 NOT BETWEEN 1 AND NULL |
+--------------------------+
|                     NULL |
+--------------------------+
```