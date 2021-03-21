# Subtraction Operator (-)

## Syntax

```sql
-
```

## Description

Subtraction. The operator is also used as the unary minus for changing sign.

If both operands are integers, the result is calculated with [BIGINT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/bigint/) precision. If either integer is unsigned, the result is also an unsigned integer, unless the NO_UNSIGNED_SUBTRACTION [SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode/) is enabled, in which case the result is always signed.

For real or string operands, the operand with the highest precision determines the result precision.

## Examples

```sql
SELECT 96-9;
+------+
| 96-9 |
+------+
|   87 |
+------+

SELECT 15-17;
+-------+
| 15-17 |
+-------+
|    -2 |
+-------+

SELECT 3.66 + 1.333;
+--------------+
| 3.66 + 1.333 |
+--------------+
|        4.993 |
+--------------+
```

Unary minus:

```sql
 SELECT - (3+5);
+---------+
| - (3+5) |
+---------+
|      -8 |
+---------+
```

## See Also

- [Type Conversion](/built-in-functions/string-functions/type-conversion/)
- [Addition Operator (+)](/built-in-functions/numeric-functions/addition-operator/)
- [Multiplication Operator (*)](/built-in-functions/numeric-functions/multiplication-operator/)
- [Division Operator (/)](/built-in-functions/numeric-functions/division-operator/)