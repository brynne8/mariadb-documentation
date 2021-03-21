# BOOLEAN

## Syntax

```sql
BOOL, BOOLEAN
```

## Description

These types are synonyms for [TINYINT(1)](/kb/en/sql_language-data_types-tinyint/). 
A value of zero is considered false. Non-zero values are considered true.

However, the values TRUE and FALSE are merely aliases for 1 and 0. See [Boolean Literals](/sql-statements-structure/sql-language-structure/sql-language-structure-boolean-literals/), as well as the [IS operator](/sql-statements-structure/operators/comparison-operators/is/) for testing values against a boolean.

## Examples

```sql
CREATE TABLE boo (i BOOLEAN);

DESC boo;
+-------+------------+------+-----+---------+-------+
| Field | Type       | Null | Key | Default | Extra |
+-------+------------+------+-----+---------+-------+
| i     | tinyint(1) | YES  |     | NULL    |       |
+-------+------------+------+-----+---------+-------+
```

```sql
SELECT IF(0, 'true', 'false');
+------------------------+
| IF(0, 'true', 'false') |
+------------------------+
| false                  |
+------------------------+

SELECT IF(1, 'true', 'false');
+------------------------+
| IF(1, 'true', 'false') |
+------------------------+
| true                   |
+------------------------+

SELECT IF(2, 'true', 'false');
+------------------------+
| IF(2, 'true', 'false') |
+------------------------+
| true                   |
+------------------------+
```

TRUE and FALSE as aliases for 1 and 0:

```sql
SELECT IF(0 = FALSE, 'true', 'false');

+--------------------------------+
| IF(0 = FALSE, 'true', 'false') |
+--------------------------------+
| true                           |
+--------------------------------+

SELECT IF(1 = TRUE, 'true', 'false');
+-------------------------------+
| IF(1 = TRUE, 'true', 'false') |
+-------------------------------+
| true                          |
+-------------------------------+

SELECT IF(2 = TRUE, 'true', 'false');
+-------------------------------+
| IF(2 = TRUE, 'true', 'false') |
+-------------------------------+
| false                         |
+-------------------------------+

SELECT IF(2 = FALSE, 'true', 'false');
+--------------------------------+
| IF(2 = FALSE, 'true', 'false') |
+--------------------------------+
| false                          |
+--------------------------------+
```

The last two statements display the results shown because 2 is equal
to neither 1 nor 0.

## See Also

- [Boolean Literals](/sql-statements-structure/sql-language-structure/sql-language-structure-boolean-literals/)
- [IS operator](/sql-statements-structure/operators/comparison-operators/is/)