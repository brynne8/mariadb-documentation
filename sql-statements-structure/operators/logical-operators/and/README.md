# &amp;&amp;

## Syntax

```sql
AND, &&
```

## Description

Logical AND. Evaluates to 1 if all operands are non-zero and not NULL,
to 0 if one or more operands are 0, otherwise NULL is returned.

For this operator, [short-circuit evaluation](/kb/en/operator-precedence/#short-circuit-evaluation) can be used.

## Examples

```sql
SELECT 1 && 1;
+--------+
| 1 && 1 |
+--------+
|      1 |
+--------+

SELECT 1 && 0;
+--------+
| 1 && 0 |
+--------+
|      0 |
+--------+

SELECT 1 && NULL;
+-----------+
| 1 && NULL |
+-----------+
|      NULL |
+-----------+

SELECT 0 && NULL;
+-----------+
| 0 && NULL |
+-----------+
|         0 |
+-----------+

SELECT NULL && 0;
+-----------+
| NULL && 0 |
+-----------+
|         0 |
+-----------+
```