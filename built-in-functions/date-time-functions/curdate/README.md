# CURDATE

## Syntax

```sql
CURDATE()
```

## Description

Returns the current date as a value in 'YYYY-MM-DD' or YYYYMMDD
format, depending on whether the function is used in a string or
numeric context.

## Examples

```sql
SELECT CURDATE();
+------------+
| CURDATE()  |
+------------+
| 2019-03-05 |
+------------+
```

In a numeric context (note this is not performing date calculations):

```sql
SELECT CURDATE() +0;
+--------------+
| CURDATE() +0 |
+--------------+
|     20190305 |
+--------------+
```

Data calculation:

```sql
SELECT CURDATE() - INTERVAL 5 DAY;
+----------------------------+
| CURDATE() - INTERVAL 5 DAY |
+----------------------------+
| 2019-02-28                 |
+----------------------------+
```