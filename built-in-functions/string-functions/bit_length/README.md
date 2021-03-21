# BIT_LENGTH

## Syntax

```sql
BIT_LENGTH(str)
```

## Description

Returns the length of the given string argument in bits. If the argument is not a string, it will be converted to string. If the argument is `NULL`, it returns `NULL`.

## Examples

```sql
SELECT BIT_LENGTH('text');
+--------------------+
| BIT_LENGTH('text') |
+--------------------+
|                 32 |
+--------------------+
```

```sql
SELECT BIT_LENGTH('');
+----------------+
| BIT_LENGTH('') |
+----------------+
|              0 |
+----------------+
```

## Compatibility

PostgreSQL and Sybase support BIT_LENGTH().