# BIT_OR

## Syntax

```sql
BIT_OR(expr) [over_clause]
```

## Description

Returns the bitwise OR of all bits in `expr`. The calculation is performed with 64-bit ([BIGINT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/bigint)) precision. It is an [aggregate function](/built-in-functions/aggregate-functions), and so can be used with the [GROUP BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/group-by) clause.

If no rows match, `BIT_OR` will return a value with all bits set to `0`. NULL values have no effect on the result unless all results are NULL, which is treated as no match.

From [MariaDB 10.2.0](/kb/en/mariadb-1020-release-notes/), `BIT_OR` can be used as a [window function](/built-in-functions/special-functions/window-functions) with the addition of the <em>over_clause</em>.

## Examples

```sql
CREATE TABLE vals (x INT);

INSERT INTO vals VALUES(111),(110),(100);

SELECT BIT_AND(x), BIT_OR(x), BIT_XOR(x) FROM vals;
+------------+-----------+------------+
| BIT_AND(x) | BIT_OR(x) | BIT_XOR(x) |
+------------+-----------+------------+
|        100 |       111 |        101 |
+------------+-----------+------------+
```

As an [aggregate function](/built-in-functions/aggregate-functions):

```sql
CREATE TABLE vals2 (category VARCHAR(1), x INT);

INSERT INTO vals2 VALUES
  ('a',111),('a',110),('a',100),
  ('b','000'),('b',001),('b',011);

SELECT category, BIT_AND(x), BIT_OR(x), BIT_XOR(x) 
  FROM vals GROUP BY category;
+----------+------------+-----------+------------+
| category | BIT_AND(x) | BIT_OR(x) | BIT_XOR(x) |
+----------+------------+-----------+------------+
| a        |        100 |       111 |        101 |
| b        |          0 |        11 |         10 |
+----------+------------+-----------+------------+
```

No match:

```sql
SELECT BIT_OR(NULL);
+--------------+
| BIT_OR(NULL) |
+--------------+
|            0 |
+--------------+
```

## See Also

- [BIT_AND](/built-in-functions/aggregate-functions/bit_and)
- [BIT_XOR](/built-in-functions/aggregate-functions/bit_xor)