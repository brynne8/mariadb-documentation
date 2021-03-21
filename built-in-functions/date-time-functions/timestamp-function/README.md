# TIMESTAMP FUNCTION

## Syntax

```sql
TIMESTAMP(expr), TIMESTAMP(expr1,expr2)
```

## Description

With a single argument, this function returns the date or datetime
expression `expr` as a datetime value. With two arguments, it adds the
time expression `expr2` to the date or datetime expression `expr1` and
returns the result as a datetime value.

## Examples

```sql
SELECT TIMESTAMP('2003-12-31');
+-------------------------+
| TIMESTAMP('2003-12-31') |
+-------------------------+
| 2003-12-31 00:00:00     |
+-------------------------+

SELECT TIMESTAMP('2003-12-31 12:00:00','6:30:00');
+--------------------------------------------+
| TIMESTAMP('2003-12-31 12:00:00','6:30:00') |
+--------------------------------------------+
| 2003-12-31 18:30:00                        |
+--------------------------------------------+
```