# BIT_COUNT

## Syntax

```sql
BIT_COUNT(N)
```

## Description

Returns the number of bits that are set in the argument N.

## Examples

```sql
SELECT BIT_COUNT(29), BIT_COUNT(b'101010');
+---------------+----------------------+
| BIT_COUNT(29) | BIT_COUNT(b'101010') |
+---------------+----------------------+
|             4 |                    3 |
+---------------+----------------------+
```