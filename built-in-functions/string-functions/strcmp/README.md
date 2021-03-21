# STRCMP

## Syntax

```sql
STRCMP(expr1,expr2)
```

## Description

`STRCMP()` returns `0` if the strings are the same, `-1` if the first
argument is smaller than the second according to the current sort order,
and `1` otherwise.

## Examples

```sql
SELECT STRCMP('text', 'text2');
+-------------------------+
| STRCMP('text', 'text2') |
+-------------------------+
|                      -1 |
+-------------------------+

SELECT STRCMP('text2', 'text');
+-------------------------+
| STRCMP('text2', 'text') |
+-------------------------+
|                       1 |
+-------------------------+

SELECT STRCMP('text', 'text');
+------------------------+
| STRCMP('text', 'text') |
+------------------------+
|                      0 |
+------------------------+
```