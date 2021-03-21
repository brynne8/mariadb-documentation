# MOD

## Syntax

```sql
MOD(N,M), N % M, N MOD M
```

## Description

Modulo operation. Returns the remainder of N divided by M. See also [Modulo Operator](/built-in-functions/numeric-functions/modulo-operator/).

If the `ERROR_ON_DIVISION_BY_ZERO` [SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode/) is used, any number modulus zero produces an error. Otherwise, it returns NULL.

The integer part of a division can be obtained using [DIV](/built-in-functions/numeric-functions/div/).

## Examples

```sql
SELECT 1042 % 50;
+-----------+
| 1042 % 50 |
+-----------+
|        42 |
+-----------+

SELECT MOD(234, 10);
+--------------+
| MOD(234, 10) |
+--------------+
|            4 |
+--------------+

SELECT 253 % 7;
+---------+
| 253 % 7 |
+---------+
|       1 |
+---------+

SELECT MOD(29,9);
+-----------+
| MOD(29,9) |
+-----------+
|         2 |
+-----------+

SELECT 29 MOD 9;
+----------+
| 29 MOD 9 |
+----------+
|        2 |
+----------+
```