# ||

## Syntax

```sql
OR, ||
```

## Description

Logical OR. When both operands are non-NULL, the result is 1 if any
operand is non-zero, and 0 otherwise. With a NULL operand, the result
is 1 if the other operand is non-zero, and NULL otherwise. If both
operands are NULL, the result is NULL.

For this operator, [short-circuit evaluation](/kb/en/operator-precedence/#short-circuit-evaluation) can be used.

Note that, if the `PIPES_AS_CONCAT` [SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode) is set, `||` is used as a string concatenation operator. This means that `a || b` is the same as `CONCAT(a,b)`. See [CONCAT()](/built-in-functions/string-functions/concat) for details.

### Oracle Mode

##### MariaDB starting with [10.3](/kb/en/what-is-mariadb-103/)

In [Oracle mode from MariaDB 10.3](/kb/en/sql_modeoracle-from-mariadb-103/#null-handling), `||` ignores [NULL](null).

## Examples

```sql
SELECT 1 || 1;
+--------+
| 1 || 1 |
+--------+
|      1 |
+--------+

SELECT 1 || 0;
+--------+
| 1 || 0 |
+--------+
|      1 |
+--------+

SELECT 0 || 0;
+--------+
| 0 || 0 |
+--------+
|      0 |
+--------+

SELECT 0 || NULL;
+-----------+
| 0 || NULL |
+-----------+
|      NULL |
+-----------+

SELECT 1 || NULL;
+-----------+
| 1 || NULL |
+-----------+
|         1 |
+-----------+
```

In [Oracle mode](/kb/en/sql_modeoracle-from-mariadb-103/#null-handling), from [MariaDB 10.3](/kb/en/what-is-mariadb-103/):

```sql
SELECT 0 || NULL;
+-----------+
| 0 || NULL |
+-----------+
| 0         |
+-----------+
```

## See Also

- [Oracle mode from MariaDB 10.3](/kb/en/sql_modeoracle-from-mariadb-103/#null-handling)