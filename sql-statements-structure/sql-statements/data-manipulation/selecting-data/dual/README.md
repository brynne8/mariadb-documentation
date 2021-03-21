# DUAL

## Description

You are allowed to specify <code class="highlight fixed" style="white-space:pre-wrap">DUAL</code> as a dummy table name in
situations where no tables are referenced, such as the following [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/) statement:

```sql
SELECT 1 + 1 FROM DUAL;
+-------+
| 1 + 1 |
+-------+
|     2 |
+-------+
```

<code class="highlight fixed" style="white-space:pre-wrap">DUAL</code> is purely for the convenience of people who require
 that all <code class="highlight fixed" style="white-space:pre-wrap">SELECT</code> statements should have
 <code class="highlight fixed" style="white-space:pre-wrap">FROM</code> and possibly other clauses. MariaDB ignores the
 clauses. MariaDB does not require <code class="highlight fixed" style="white-space:pre-wrap">FROM DUAL</code> if no tables
 are referenced.

FROM DUAL could be used when you only SELECT computed values, but require a WHERE clause, perhaps to test that a script correctly handles empty resultsets:

```sql
SELECT 1 FROM DUAL WHERE FALSE;
Empty set (0.00 sec)
```

## See Also

- [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/)