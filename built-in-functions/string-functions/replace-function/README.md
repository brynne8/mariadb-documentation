# REPLACE Function

## Syntax

```sql
REPLACE(str,from_str,to_str)
```

## Description

Returns the string `str` with all occurrences of the string `from_str`
replaced by the string `to_str`. REPLACE() performs a case-sensitive
match when searching for `from_str`.

## Examples

```sql
SELECT REPLACE('www.mariadb.org', 'w', 'Ww');
+---------------------------------------+
| REPLACE('www.mariadb.org', 'w', 'Ww') |
+---------------------------------------+
| WwWwWw.mariadb.org                    |
+---------------------------------------+
```