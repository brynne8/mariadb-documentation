# MINUTE

## Syntax

```sql
MINUTE(time)
```

## Description

Returns the minute for <em>time</em>, in the range 0 to 59.

## Examples

```sql
SELECT MINUTE('2013-08-03 11:04:03');
+-------------------------------+
| MINUTE('2013-08-03 11:04:03') |
+-------------------------------+
|                             4 |
+-------------------------------+

 SELECT MINUTE ('23:12:50');
+---------------------+
| MINUTE ('23:12:50') |
+---------------------+
|                  12 |
+---------------------+
```