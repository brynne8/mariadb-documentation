# Assignment Operator (=)

## Syntax

```sql
identifier = expr
```

## Description

The equal sign is used as both an assignment operator in certain contexts, and as a [comparison operator](/sql-statements-structure/operators/comparison-operators/equal/). When used as assignment operator, the value on the right is assigned to the variable (or column, in some contexts) on the left.

Since its use can be ambiguous, unlike the [:= assignment operator](/kb/en/assignment-operator/), the <em>`=`</em> assignment operator cannot be used in all contexts, and is only valid as part of a [SET](/sql-statements-structure/sql-statements/administrative-sql-statements/set-commands/set/) statement, or the SET clause of an [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update/) statement

This operator works with both [user-defined variables](/sql-statements-structure/sql-language-structure/user-defined-variables/) and [local variables](/programming-customizing-mariadb/programmatic-compound-statements/declare-variable/).

## Examples

```sql
UPDATE table_name SET x = 2 WHERE x > 100;
```

```sql
SET @x = 1, @y := 2;
```