# RAND

## Syntax

```sql
RAND(), RAND(N)
```

## Description

Returns a random [DOUBLE](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/double) precision floating point value v in the range 0 &lt;= v &lt; 1.0. If
a constant integer argument N is specified, it is used as the seed
value, which produces a repeatable sequence of column values. In the example below, note that the sequences of values produced by
RAND(3) is the same both places where it occurs.

In a WHERE clause, RAND() is evaluated each time the WHERE is executed.

Statements using the RAND() function are not [safe for statement-based replication](/kb/en/unsafe-statements-for-replication/).

## Practical uses

The expression to get a random integer from a given range is the following:

```sql
FLOOR(min_value + RAND() * (max_value - min_value +1))
```

RAND() is often used to read random rows from a table, as follows:

```sql
SELECT * FROM my_table ORDER BY RAND() LIMIT 10;
```

Note, however, that this technique should never be used on a large table as it will be extremely slow. MariaDB will read all rows in the table, generate a random value for each of them, order them, and finally will apply the LIMIT clause.

## Examples

```sql
CREATE TABLE t (i INT);

INSERT INTO t VALUES(1),(2),(3);

SELECT i, RAND() FROM t;
+------+-------------------+
| i    | RAND()            |
+------+-------------------+
|    1 | 0.255651095188829 |
|    2 | 0.833920199269355 |
|    3 |  0.40264774151393 |
+------+-------------------+

SELECT i, RAND(3) FROM t;
+------+-------------------+
| i    | RAND(3)           |
+------+-------------------+
|    1 |  0.90576975597606 |
|    2 | 0.373079058130345 |
|    3 | 0.148086053457191 |
+------+-------------------+

SELECT i, RAND() FROM t;
+------+-------------------+
| i    | RAND()            |
+------+-------------------+
|    1 | 0.511478140495232 |
|    2 | 0.349447508668012 |
|    3 | 0.212803152588013 |
+------+-------------------+
```

Using the same seed, the same sequence will be returned:

```sql
SELECT i, RAND(3) FROM t;
+------+-------------------+
| i    | RAND(3)           |
+------+-------------------+
|    1 |  0.90576975597606 |
|    2 | 0.373079058130345 |
|    3 | 0.148086053457191 |
+------+-------------------+
```

Generating a random number from 5 to 15:

```sql
SELECT FLOOR(5 + (RAND() * 11));
```

## See Also

- [Techniques for Efficiently Finding a Random Row](/kb/en/techniques-for-efficiently-finding-a-random-row/)
- [rand_seed1 and rand_seed2 system variables](/kb/en/server-system-variables/#rand_seed1)