# Multiplication Operator (*)

## Syntax

```sql
*
```

## Description

Multiplication operator.

## Examples

```sql
SELECT 7*6;
+-----+
| 7*6 |
+-----+
|  42 |
+-----+

SELECT 1234567890*9876543210;
+-----------------------+
| 1234567890*9876543210 |
+-----------------------+
|  -6253480962446024716 |
+-----------------------+

SELECT 18014398509481984*18014398509481984.0;
+---------------------------------------+
| 18014398509481984*18014398509481984.0 |
+---------------------------------------+
|   324518553658426726783156020576256.0 |
+---------------------------------------+

SELECT 18014398509481984*18014398509481984;
+-------------------------------------+
| 18014398509481984*18014398509481984 |
+-------------------------------------+
|                                   0 |
+-------------------------------------+
```

## See Also

- [Type Conversion](/built-in-functions/string-functions/type-conversion/)
- [Addition Operator (+)](/built-in-functions/numeric-functions/addition-operator/)
- [Subtraction Operator (-)](/sql-statements-structure/operators/arithmetic-operators/subtraction-operator-/)
- [Division Operator (/)](/built-in-functions/numeric-functions/division-operator/)