# ~

## Syntax

```sql
~
```

## Description

Bitwise NOT. Converts the value to 4 bytes binary and inverts all bits.

## Examples

```sql
SELECT 3 & ~1;
+--------+
| 3 & ~1 |
+--------+
|      2 |
+--------+

SELECT 5 & ~1;
+--------+
| 5 & ~1 |
+--------+
|      4 |
+--------+
```