# PI

## Syntax

```sql
PI()
```

## Description

Returns the value of π (pi). The default number of decimal places
displayed is six, but MariaDB uses the full double-precision value
internally.

## Examples

```sql
SELECT PI();
+----------+
| PI()     |
+----------+
| 3.141593 |
+----------+

SELECT PI()+0.0000000000000000000000;
+-------------------------------+
| PI()+0.0000000000000000000000 |
+-------------------------------+
|      3.1415926535897931159980 |
+-------------------------------+
```