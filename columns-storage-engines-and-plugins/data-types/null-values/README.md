# NULL Values

NULL represents an unknown value. It is <em>not</em> an empty string (by default), or a zero value. These are all valid values, and are not NULLs.

When a table is [created](/sql-statements-structure/sql-statements/data-definition/create/create-table) or the format [altered](/sql-statements-structure/sql-statements/data-definition/alter/alter-table), columns can be specified as accepting NULL values, or not accepting them, with the `NULL` and `NOT NULL` clauses respectively.

For example, a customer table could contain dates of birth. For some customers, this information is unknown, so the value could be NULL.

The same system could allocate a customer ID for each customer record, and in this case a NULL value would not be permitted.

```sql
CREATE TABLE customer (
 id INT NOT NULL, 
 date_of_birth DATE NULL
...
)
```

[User-defined variables](/sql-statements-structure/sql-language-structure/user-defined-variables) are NULL until a value is explicitly assigned.

[Stored routines](/kb/en/stored-programs-and-views/) parameters and [local variables](/programming-customizing-mariadb/programmatic-compound-statements/declare-variable) can always be set to NULL. If no DEFAULT value is specified for a local variable, its initial value will be NULL. If no value is assigned to an OUT parameter in a stored procedure, NULL is assigned at the end of the procedure.

## Syntax

The case of `NULL` is not relevant. `\N` (uppercase) is an alias for `NULL`.

The [IS](/sql-statements-structure/operators/comparison-operators/is) operator accepts `UNKNOWN` as an alias for `NULL`, which is meant for [boolean contexts](/sql-statements-structure/sql-language-structure/sql-language-structure-boolean-literals).

## Comparison Operators

NULL values cannot be used with most [comparison operators](/sql-statements-structure/operators/comparison-operators). For example, [=](/sql-statements-structure/operators/comparison-operators/equal), [&gt;](/sql-statements-structure/operators/comparison-operators/greater-than), [&gt;=](/sql-statements-structure/operators/comparison-operators/greater-than-or-equal), [&lt;=](/sql-statements-structure/operators/comparison-operators/less-than-or-equal), [&lt;](/sql-statements-structure/operators/comparison-operators/less-than), or [!=](/sql-statements-structure/operators/comparison-operators/not-equal) cannot be used, as any comparison with a NULL always returns a NULL value, never true (1) or false (0).

```sql
SELECT NULL = NULL;
+-------------+
| NULL = NULL |
+-------------+
|        NULL |
+-------------+

SELECT 99 = NULL;
+-----------+
| 99 = NULL |
+-----------+
|      NULL |
+-----------+
```

To overcome this, certain operators are specifically designed for use with NULL values. To cater for testing equality between two values that may contain NULLs, there's [&lt;=&gt;](/sql-statements-structure/operators/comparison-operators/null-safe-equal), NULL-safe equal.

```sql
SELECT 99 <=> NULL, NULL <=> NULL;
+-------------+---------------+
| 99 <=> NULL | NULL <=> NULL |
+-------------+---------------+
|           0 |             1 |
+-------------+---------------+
```

Other operators for working with NULLs include [IS NULL](/sql-statements-structure/operators/comparison-operators/is-null) and [IS NOT NULL](/sql-statements-structure/operators/comparison-operators/is-not-null), [ISNULL](/sql-statements-structure/operators/comparison-operators/isnull) (for testing an expression) and [COALESCE](/sql-statements-structure/operators/comparison-operators/coalesce) (for returning the first non-NULL parameter).

## Ordering

When you order by a field that may contain NULL values, any NULLs are considered to have the lowest value. So ordering in DESC order will see the NULLs appearing last. To force NULLs to be regarded as highest values, one can add another column which has a higher value when the main field is NULL. Example:

```sql
SELECT col1 FROM tab ORDER BY ISNULL(col1), col1;
```

Descending order, with NULLs first:

```sql
SELECT col1 FROM tab ORDER BY IF(col1 IS NULL, 0, 1), col1 DESC;
```

All NULL values are also regarded as equivalent for the purposes of the DISTINCT and GROUP BY clauses.

## Functions

In most cases, functions will return NULL if any of the parameters are NULL. There are also functions specifically for handling NULLs. These include [IFNULL()](/built-in-functions/control-flow-functions/ifnull),  [NULLIF()](/built-in-functions/control-flow-functions/nullif) and [COALESCE()](/sql-statements-structure/operators/comparison-operators/coalesce).

```sql
SELECT IFNULL(1,0); 
+-------------+
| IFNULL(1,0) |
+-------------+
|           1 |
+-------------+

SELECT IFNULL(NULL,10);
+-----------------+
| IFNULL(NULL,10) |
+-----------------+
|              10 |
+-----------------+

SELECT COALESCE(NULL,NULL,1);
+-----------------------+
| COALESCE(NULL,NULL,1) |
+-----------------------+
|                     1 |
+-----------------------+
```

Aggregate functions, such as [SUM](/built-in-functions/aggregate-functions/sum) and [AVG](/built-in-functions/aggregate-functions/avg) ignore NULLs.

```sql
CREATE TABLE t(x INT);

INSERT INTO t VALUES (1),(9),(NULL);

SELECT SUM(x) FROM t;
+--------+
| SUM(x) |
+--------+
|     10 |
+--------+

SELECT AVG(x) FROM t;
+--------+
| AVG(x) |
+--------+
| 5.0000 |
+--------+
```

The one exception is [COUNT(*)](/built-in-functions/aggregate-functions/count), which counts rows, and doesn't look at whether a value is NULL or not. Compare for example, COUNT(x), which ignores the NULL, and COUNT(*), which counts it:

```sql
SELECT COUNT(x) FROM t;
+----------+
| COUNT(x) |
+----------+
|        2 |
+----------+

SELECT COUNT(*) FROM t;
+----------+
| COUNT(*) |
+----------+
|        3 |
+----------+
```

## AUTO_INCREMENT, TIMESTAMP and Virtual Columns

MariaDB handles NULL values in a special way if the field is an [AUTO_INCREMENT](/columns-storage-engines-and-plugins/data-types/auto_increment), a [TIMESTAMP](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/timestamp) or a [virtual column](/kb/en/virtual-columns/). Inserting a NULL value into a numeric AUTO_INCREMENT column will result in the next number in the auto increment sequence being inserted instead. This technique is frequently used with AUTO_INCREMENT fields, which are left to take care of themselves.

```sql
CREATE TABLE t2(id INT PRIMARY KEY AUTO_INCREMENT, letter CHAR(1));

INSERT INTO t2(letter) VALUES ('a'),('b');

SELECT * FROM t2;
+----+--------+
| id | letter |
+----+--------+
|  1 | a      |
|  2 | b      |
+----+--------+
```

Similarly, if a NULL value is assigned to a TIMESTAMP field, the current date and time is assigned instead.

```sql
CREATE TABLE t3 (x INT, ts TIMESTAMP);

INSERT INTO t3(x) VALUES (1),(2);
```

After a pause,

```sql
INSERT INTO t3(x) VALUES (3);

SELECT* FROM t3;
+------+---------------------+
| x    | ts                  |
+------+---------------------+
|    1 | 2013-09-05 10:14:18 |
|    2 | 2013-09-05 10:14:18 |
|    3 | 2013-09-05 10:14:29 |
+------+---------------------+
```

If a `NULL` is assigned to a `VIRTUAL` or `PERSISTENT` column, the default value is assigned instead.

```sql
CREATE TABLE virt (c INT, v INT AS (c+10) PERSISTENT) ENGINE=InnoDB;

INSERT INTO virt VALUES (1, NULL);

SELECT c, v FROM virt;
+------+------+
| c    | v    |
+------+------+
|    1 |   11 |
+------+------+
```

In all these special cases, `NULL` is equivalent to the `DEFAULT` keyword.

## Inserting

If a NULL value is single-row inserted into a column declared as NOT NULL, an error will be returned. However, if the [SQL mode](/kb/en/sql_mode/) is not [strict](/kb/en/sql-mode/#strict-mode) (default until [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/)), if a NULL value is multi-row inserted into a column declared as NOT NULL, the implicit default for the column type will be inserted (and NOT the default value in the table definition). The implicit defaults are an empty string for string types, and the zero value for numeric, date and time types.

Since [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/), by default both cases will result in an error.

### Examples

```sql
CREATE TABLE nulltest (
  a INT(11), 
  x VARCHAR(10) NOT NULL DEFAULT 'a', 
  y INT(11) NOT NULL DEFAULT 23
);
```

Single-row insert:

```sql
INSERT INTO nulltest (a,x,y) VALUES (1,NULL,NULL);
ERROR 1048 (23000): Column 'x' cannot be null
```

Multi-row insert with [SQL mode](/kb/en/sql_mode/) not [strict](/kb/en/sql-mode/#strict-mode) (default until [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/)):

```sql
show variables like 'sql_mode%';
+---------------+--------------------------------------------+
| Variable_name | Value                                      |
+---------------+--------------------------------------------+
| sql_mode      | NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION |
+---------------+--------------------------------------------+

INSERT INTO nulltest (a,x,y) VALUES (1,NULL,NULL),(2,NULL,NULL); 
Query OK, 2 rows affected, 4 warnings (0.08 sec)
Records: 2  Duplicates: 0  Warnings: 4
```

The specified defaults have not been used; rather, the implicit column type defaults have been inserted

```sql
SELECT * FROM nulltest;
+------+---+---+
| a    | x | y |
+------+---+---+
|    1 |   | 0 |
|    2 |   | 0 |
+------+---+---+
```

## Primary Keys and UNIQUE Indexes

UNIQUE indexes can contain multiple NULL values.

Primary keys are never nullable.

<br>

##### MariaDB starting with [10.3](/kb/en/what-is-mariadb-103/)

## Oracle Compatibility

In [Oracle mode](/kb/en/sql_modeoracle-from-mariadb-103/#null-handling), NULL can be used as a statement:

```sql
IF a=10 THEN NULL; ELSE NULL; END IF
```

In [Oracle mode](/kb/en/sql_modeoracle-from-mariadb-103/#null-handling), [CONCAT](/built-in-functions/string-functions/concat) and the [Logical OR operator ||](/sql-statements-structure/operators/logical-operators/or) ignore [NULL](null).

When setting [sql_mode=EMPTY_STRING_IS_NULL](/mariadb-administration/variables-and-modes/sql-mode), empty strings and NULLs are the same thing. For example:

```sql
SET sql_mode=EMPTY_STRING_IS_NULL;
SELECT '' IS NULL; -- returns TRUE
INSERT INTO t1 VALUES (''); -- inserts NULL
```

## See Also

- [Primary Keys with Nullable Columns](/replication/optimization-and-tuning/optimization-and-indexes/primary-keys-with-nullable-columns)
- [IS NULL operator](/sql-statements-structure/operators/comparison-operators/is-null)
- [IS NOT NULL operator](/sql-statements-structure/operators/comparison-operators/is-not-null)
- [ISNULL function](/sql-statements-structure/operators/comparison-operators/isnull)
- [COALESCE function](/sql-statements-structure/operators/comparison-operators/coalesce)
- [IFNULL function](/built-in-functions/control-flow-functions/ifnull)
- [NULLIF function](/built-in-functions/control-flow-functions/nullif)
- [CONNECT data types](/kb/en/connect-data-types/#null-handling)
- [Oracle mode from MariaDB 10.3](/kb/en/sql_modeoracle-from-mariadb-103/#null-handling)