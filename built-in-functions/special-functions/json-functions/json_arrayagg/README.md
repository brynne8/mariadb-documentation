# JSON_ARRAYAGG

##### MariaDB starting with [10.5.0](/kb/en/mariadb-1050-release-notes/)

JSON_ARRAYAGG was added in [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/).

## Syntax

```sql
JSON_ARRAYAGG(column_or_expression)
```

## Description

`JSON_ARRAYAGG` returns a JSON array containing an element for each value in a given set of JSON or SQL values. It acts on a column or an expression that evaluates to a single value.

Returns NULL in the case of an error, or if the result contains no rows.

`JSON_ARRAYAGG` cannot currently be used as a [window function](/built-in-functions/special-functions/window-functions/).

The full syntax is as follows:

```sql
JSON_ARRAYAGG([DISTINCT] expr [,expr ...]
             [ORDER BY {unsigned_integer | col_name | expr}
                 [ASC | DESC] [,col_name ...]]
             [LIMIT {[offset,] row_count | row_count OFFSET offset}])
```

## Examples

```sql
CREATE TABLE t1 (a INT, b INT);

INSERT INTO t1 VALUES (1, 1),(2, 1), (1, 1),(2, 1), (3, 2),(2, 2),(2, 2),(2, 2);

SELECT JSON_ARRAYAGG(a), JSON_ARRAYAGG(b) FROM t1;
+-------------------+-------------------+
| JSON_ARRAYAGG(a)  | JSON_ARRAYAGG(b)  |
+-------------------+-------------------+
| [1,2,1,2,3,2,2,2] | [1,1,1,1,2,2,2,2] |
+-------------------+-------------------+

SELECT JSON_ARRAYAGG(a), JSON_ARRAYAGG(b) FROM t1 GROUP BY b;
+------------------+------------------+
| JSON_ARRAYAGG(a) | JSON_ARRAYAGG(b) |
+------------------+------------------+
| [1,2,1,2]        | [1,1,1,1]        |
| [3,2,2,2]        | [2,2,2,2]        |
+------------------+------------------+
```