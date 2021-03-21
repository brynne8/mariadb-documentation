# LOG

## Syntax

```sql
LOG(X), LOG(B,X)
```

## Description

If called with one parameter, this function returns the natural
logarithm of X. If X is less than or equal to 0, then NULL is
returned.

If called with two parameters, it returns the logarithm of X to the base B. If  B is &lt;= 1 or X &lt;= 0, the function returns NULL.

If any argument is `NULL`, the function returns `NULL`.

The inverse of this function (when called with a single argument) is
the [EXP()](/built-in-functions/numeric-functions/exp/) function.

## Examples

LOG(X):

```sql
SELECT LOG(2);
+-------------------+
| LOG(2)            |
+-------------------+
| 0.693147180559945 |
+-------------------+

SELECT LOG(-2);
+---------+
| LOG(-2) |
+---------+
|    NULL |
+---------+
```

LOG(B,X)

```sql
SELECT LOG(2,16);
+-----------+
| LOG(2,16) |
+-----------+
|         4 |
+-----------+

SELECT LOG(3,27);
+-----------+
| LOG(3,27) |
+-----------+
|         3 |
+-----------+

SELECT LOG(3,1);
+----------+
| LOG(3,1) |
+----------+
|        0 |
+----------+

SELECT LOG(3,0);
+----------+
| LOG(3,0) |
+----------+
|     NULL |
+----------+
```