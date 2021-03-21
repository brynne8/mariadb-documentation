# Division Operator (/)

## Syntax

```sql
/
```

## Description

Division operator. Dividing by zero will return NULL. By default, returns four digits after the decimal. This is determined by the server system variable [div_precision_increment](/kb/en/server-system-variables/#div_precision_increment) which by default is four. It can be set from 0 to 30.

Dividing by zero returns `NULL`. If the `ERROR_ON_DIVISION_BY_ZERO` [SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode/) is used (the default since [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/)), a division by zero also produces a warning.

## Examples

```sql
SELECT 4/5;
+--------+
| 4/5    |
+--------+
| 0.8000 |
+--------+

SELECT 300/(2-2);
+-----------+
| 300/(2-2) |
+-----------+
|      NULL |
+-----------+

SELECT 300/7;
+---------+
| 300/7   |
+---------+
| 42.8571 |
+---------+
```

Changing [div_precision_increment](/kb/en/server-system-variables/#div_precision_increment) for the session from the default of four to six:

```sql
SET div_precision_increment = 6;

SELECT 300/7;
+-----------+
| 300/7     |
+-----------+
| 42.857143 |
+-----------+

SELECT 300/7;
+-----------+
| 300/7     |
+-----------+
| 42.857143 |
+-----------+
```

## See Also

- [Type Conversion](/built-in-functions/string-functions/type-conversion/)
- [Addition Operator (+)](/built-in-functions/numeric-functions/addition-operator/)
- [Subtraction Operator (-)](/sql-statements-structure/operators/arithmetic-operators/subtraction-operator-/)
- [Multiplication Operator (*)](/built-in-functions/numeric-functions/multiplication-operator/)