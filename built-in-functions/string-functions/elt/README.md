# ELT

## Syntax

```sql
ELT(N, str1[, str2, str3,...])
```

## Description

Takes a numeric argument and a series of string arguments. Returns the string that corresponds to the given numeric position.  For instance, it returns `str1` if `N` is 1, `str2` if `N` is 2, and so on.  If the numeric argument is a [FLOAT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/float/), MariaDB rounds it to the nearest [INTEGER](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/int/).  If the numeric argument is less than 1, greater than the total number of arguments, or not a number, `ELT()` returns `NULL`.  It must have at least two arguments.

It is complementary to the [FIELD()](/built-in-functions/string-functions/field/) function.

## Examples

```sql
SELECT ELT(1, 'ej', 'Heja', 'hej', 'foo');
+------------------------------------+
| ELT(1, 'ej', 'Heja', 'hej', 'foo') |
+------------------------------------+
| ej                                 |
+------------------------------------+

SELECT ELT(4, 'ej', 'Heja', 'hej', 'foo');
+------------------------------------+
| ELT(4, 'ej', 'Heja', 'hej', 'foo') |
+------------------------------------+
| foo                                |
+------------------------------------+
```

## See also

- [FIND_IN_SET()](/built-in-functions/string-functions/find_in_set/) function. Returns the position of a string in a set of strings.
- [FIELD()](/built-in-functions/string-functions/field/) function. Returns the index position of a string in a list.