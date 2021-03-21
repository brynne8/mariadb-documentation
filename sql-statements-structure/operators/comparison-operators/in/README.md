# IN

## Syntax

```sql
expr IN (value,...)
```

## Description

Returns 1 if <em>`expr`</em> is equal to any of the values in the IN list, else
returns 0. If all values are constants, they are evaluated according
to the type of <em>`expr`</em> and sorted. The search for the item then is done
using a binary search. This means IN is very quick if the IN value
list consists entirely of constants. Otherwise, type conversion takes
place according to the rules described at [Type Conversion](/built-in-functions/string-functions/type-conversion/), but
applied to all the arguments.

If <em>`expr`</em> is NULL, IN always returns NULL. If at least one of the values in the list is NULL, and one of the comparisons is true, the result is 1. If at least one of the values in the list is NULL and none of the comparisons is true, the result is NULL.

## Examples

```sql
SELECT 2 IN (0,3,5,7);
+----------------+
| 2 IN (0,3,5,7) |
+----------------+
|              0 |
+----------------+
```

```sql
SELECT 'wefwf' IN ('wee','wefwf','weg');
+----------------------------------+
| 'wefwf' IN ('wee','wefwf','weg') |
+----------------------------------+
|                                1 |
+----------------------------------+ 
```

Type conversion:

```sql
SELECT 1 IN ('1', '2', '3');
+----------------------+
| 1 IN ('1', '2', '3') |
+----------------------+
|                    1 |
+----------------------+
```

```sql
SELECT NULL IN (1, 2, 3);
+-------------------+
| NULL IN (1, 2, 3) |
+-------------------+
|              NULL |
+-------------------+

MariaDB [(none)]> SELECT 1 IN (1, 2, NULL);
+-------------------+
| 1 IN (1, 2, NULL) |
+-------------------+
|                 1 |
+-------------------+

MariaDB [(none)]> SELECT 5 IN (1, 2, NULL);
+-------------------+
| 5 IN (1, 2, NULL) |
+-------------------+
|              NULL |
+-------------------+
```