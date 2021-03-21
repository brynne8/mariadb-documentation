# NVL2

##### MariaDB starting with [10.3](/kb/en/what-is-mariadb-103/)

The NLV2 function was introduced in [MariaDB 10.3.0](/kb/en/mariadb-1030-release-notes/).

## Syntax

```sql
NVL2(expr1,expr2,expr3)
```

## Description

The `NVL2` function returns a value based on whether a specified expression is NULL or not. If <em>expr1</em> is not NULL, then NVL2 returns <em>expr2</em>. If <em>expr1</em> is NULL, then NVL2 returns <em>expr3</em>.

## Examples

```sql
SELECT NVL2(NULL,1,2);
+----------------+
| NVL2(NULL,1,2) |
+----------------+
|              2 |
+----------------+

SELECT NVL2('x',1,2);
+---------------+
| NVL2('x',1,2) |
+---------------+
|             1 |
+---------------+
```

## See Also

- [IFNULL (or NVL)](/built-in-functions/control-flow-functions/ifnull)