# SPACE

## Syntax

```sql
SPACE(N)
```

## Description

Returns a string consisting of <em>`N`</em> space characters. If `N` is NULL, returns NULL.

## Examples

```sql
SELECT QUOTE(SPACE(6));
+-----------------+
| QUOTE(SPACE(6)) |
+-----------------+
| '      '        |
+-----------------+
```