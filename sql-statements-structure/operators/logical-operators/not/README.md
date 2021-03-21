# !

## Syntax

```sql
NOT, !
```

## Description

Logical NOT. Evaluates to 1 if the operand is 0, to 0 if the operand
is non-zero, and NOT NULL returns NULL.

By default, the `!` operator has a [higher precedence](/sql-statements-structure/operators/operator-precedence). If the `HIGH_NOT_PRECEDENCE` [SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode) flag is set, `NOT` and `!` have the same precedence.

## Examples

```sql
SELECT NOT 10;
+--------+
| NOT 10 |
+--------+
|      0 |
+--------+

SELECT NOT 0;
+-------+
| NOT 0 |
+-------+
|     1 |
+-------+

SELECT NOT NULL;
+----------+
| NOT NULL |
+----------+
|     NULL |
+----------+

SELECT ! (1+1);
+---------+
| ! (1+1) |
+---------+
|       0 |
+---------+

SELECT ! 1+1;
+-------+
| ! 1+1 |
+-------+
|     1 |
+-------+
```