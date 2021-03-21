# BENCHMARK

## Syntax

```sql
BENCHMARK(count,expr)
```

## Description

The BENCHMARK() function executes the expression `expr` repeatedly `count`
times. It may be used to time how quickly MariaDB processes the
expression. The result value is always 0. The intended use is from
within the [mysql client](/clients-utilities/mysql-client/mysql-command-line-client/), which reports query execution times.

## Examples

```sql
SELECT BENCHMARK(1000000,ENCODE('hello','goodbye'));
+----------------------------------------------+
| BENCHMARK(1000000,ENCODE('hello','goodbye')) |
+----------------------------------------------+
|                                            0 |
+----------------------------------------------+
1 row in set (0.21 sec)
```