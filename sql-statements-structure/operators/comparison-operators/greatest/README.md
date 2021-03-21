# GREATEST

## Syntax

```sql
GREATEST(value1,value2,...)
```

## Description

With two or more arguments, returns the largest (maximum-valued)
argument. The arguments are compared using the same rules as for
[LEAST()](/sql-statements-structure/operators/comparison-operators/least).

## Examples

```sql
SELECT GREATEST(2,0);
+---------------+
| GREATEST(2,0) |
+---------------+
|             2 |
+---------------+
```

```sql
SELECT GREATEST(34.0,3.0,5.0,767.0);
+------------------------------+
| GREATEST(34.0,3.0,5.0,767.0) |
+------------------------------+
|                        767.0 |
+------------------------------+
```

```sql
SELECT GREATEST('B','A','C');
+-----------------------+
| GREATEST('B','A','C') |
+-----------------------+
| C                     |
+-----------------------+
```