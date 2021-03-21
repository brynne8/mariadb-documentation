# Assignment Operator (:=)

## Syntax

```sql
var_name := expr
```

## Description

Assignment operator for assigning a value. The value on the right is assigned to the variable on left.

Unlike the [= operator](/sql-statements-structure/operators/assignment-operators/assignment-operators-assignment-operator/), `:=` can always be used to assign a value to a variable.

This operator works with both [user-defined variables](/sql-statements-structure/sql-language-structure/user-defined-variables/) and [local variables](/programming-customizing-mariadb/programmatic-compound-statements/declare-variable/).

When assigning the same value to several variables, [LAST_VALUE()](/built-in-functions/secondary-functions/information-functions/last_value/) can be useful.

## Examples

```sql
 SELECT @x := 10;
+----------+
| @x := 10 |
+----------+
|       10 |
+----------+

SELECT @x, @y := @x;
+------+----------+
| @x   | @y := @x |
+------+----------+
|   10 |       10 |
+------+----------+
```