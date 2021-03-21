# DATE FUNCTION

## Syntax

```sql
DATE(expr)
```

## Description

Extracts the date part of the date or datetime expression expr.

## Examples

```sql
SELECT DATE('2013-07-18 12:21:32');
+-----------------------------+
| DATE('2013-07-18 12:21:32') |
+-----------------------------+
| 2013-07-18                  |
+-----------------------------+
```

## Error Handling

Until [MariaDB 5.5.32](/kb/en/mariadb-5532-release-notes/), some versions of MariaDB returned `0000-00-00` when passed an invalid date. From 5.5.32, NULL is returned.