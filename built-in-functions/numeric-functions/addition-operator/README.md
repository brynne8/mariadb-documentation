# Addition Operator (+)

## Syntax

```sql
+
```

## Description

Addition.

If both operands are integers, the result is calculated with [BIGINT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/bigint) precision. If either integer is unsigned, the result is also an unsigned integer.

For real or string operands, the operand with the highest precision determines the result precision.

## Examples

```sql
SELECT 3+5;
+-----+
| 3+5 |
+-----+
|   8 |
+-----+
```

## See Also

- [Type Conversion](/built-in-functions/string-functions/type-conversion)
- [Subtraction Operator (-)](/sql-statements-structure/operators/arithmetic-operators/subtraction-operator-)
- [Multiplication Operator (*)](/built-in-functions/numeric-functions/multiplication-operator)
- [Division Operator (/)](/built-in-functions/numeric-functions/division-operator)