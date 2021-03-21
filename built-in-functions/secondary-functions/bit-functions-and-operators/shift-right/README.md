# &gt;&gt;

## Syntax

```sql
value1 >> value2
```

## Description

Converts a longlong ([BIGINT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/bigint/)) number (<em>value1</em>) to binary and shifts <em>value2</em> units to the right.

## Examples

```sql
SELECT 4 >> 2;
+--------+
| 4 >> 2 |
+--------+
|      1 |
+--------+
```