# FIELD

## Syntax

```sql
FIELD(pattern, str1[,str2,...])
```

## Description

Returns the index position of the string or number matching the given pattern.  Returns `0` in the event that none of the arguments match the pattern.  Raises an Error 1582 if not given at least two arguments.

When all arguments given to the `FIELD()` function are strings, they are treated as case-insensitive.  When all the arguments are numbers, they are treated as numbers.  Otherwise, they are treated as doubles.

If the given pattern occurs more than once, the	`FIELD()` function only returns the index of the first instance.  If the given pattern is `NULL`, the function returns `0`, as a `NULL` pattern always fails to match.

This function is complementary to the [ELT()](/built-in-functions/string-functions/elt/) function.

## Examples

```sql
SELECT FIELD('ej', 'Hej', 'ej', 'Heja', 'hej', 'foo') 
   AS 'Field Results';
+---------------+
| Field Results | 
+---------------+
|             2 |
+---------------+

SELECT FIELD('fo', 'Hej', 'ej', 'Heja', 'hej', 'foo')
   AS 'Field Results';
+---------------+
| Field Results | 
+---------------+
|             0 |
+---------------+

SELECT FIELD(1, 2, 3, 4, 5, 1) AS 'Field Results';
+---------------+
| Field Results |
+---------------+
|             5 |
+---------------+

SELECT FIELD(NULL, 2, 3) AS 'Field Results';
+---------------+
| Field Results |
+---------------+
|             0 |
+---------------+

SELECT FIELD('fail') AS 'Field Results';
Error 1582 (42000): Incorrect parameter count in call
to native function 'field'
```

## See also

- [ELT()](/built-in-functions/string-functions/elt/) function. Returns the N'th element from a set of strings.