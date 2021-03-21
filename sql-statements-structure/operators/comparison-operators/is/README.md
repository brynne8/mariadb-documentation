# IS

## Syntax

```sql
IS boolean_value
```

## Description

Tests a value against a boolean value, where `boolean_value` can be
TRUE, FALSE, or UNKNOWN.

There is an important difference between using IS TRUE or comparing a value with TRUE using `=`. When using `=`, only `1` equals to TRUE. But when using IS TRUE, all values which are logically true (like a number &gt; 1) return TRUE.

## Examples

```sql
SELECT 1 IS TRUE, 0 IS FALSE, NULL IS UNKNOWN;
+-----------+------------+-----------------+
| 1 IS TRUE | 0 IS FALSE | NULL IS UNKNOWN |
+-----------+------------+-----------------+
|         1 |          1 |               1 |
+-----------+------------+-----------------+
```

Difference between `=` and `IS TRUE`:

```sql
SELECT 2 = TRUE, 2 IS TRUE;
+----------+-----------+
| 2 = TRUE | 2 IS TRUE |
+----------+-----------+
|        0 |         1 |
+----------+-----------+
```

## See Also

- [Boolean Literals](/sql-statements-structure/sql-language-structure/sql-language-structure-boolean-literals)
- [BOOLEAN Data Type](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/boolean)