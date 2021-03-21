# FIND_IN_SET

## Syntax

```sql
FIND_IN_SET(pattern, strlist)
```

## Description

Returns the index position where the given pattern occurs in a string list.  The first argument is the pattern you want to search for.  The second argument is a string containing comma-separated variables.  If the second argument is of the [SET](/columns-storage-engines-and-plugins/data-types/string-data-types/set-data-type) data-type, the function is optimized to use bit arithmetic.

If the pattern does not occur in the string list or if the string list is an empty string, the function returns `0`.  If either argument is `NULL`, the function returns `NULL`.  The function does not return the correct result if the pattern contains a comma ("`,`") character.

## Examples

```sql
SELECT FIND_IN_SET('b','a,b,c,d') AS "Found Results";
+---------------+
| Found Results |
+---------------+
|             2 |
+---------------+
```

## See Also

- [ELT()](/built-in-functions/string-functions/elt) function. Returns the N'th element from a set of strings.