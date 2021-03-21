# CONCAT_WS

## Syntax

```sql
CONCAT_WS(separator,str1,str2,...)
```

## Description

`CONCAT_WS()` stands for Concatenate With Separator and is a special form of [CONCAT()](/built-in-functions/string-functions/concat). The first argument is the separator for the rest of the arguments. The separator is added between the strings to be concatenated. The separator can be a string, as can the rest of the arguments.

If the separator is `NULL`, the result is `NULL`; all other `NULL` values are skipped. This makes `CONCAT_WS()` suitable when you want to concatenate some values and avoid losing all information if one of them is `NULL`.

## Examples

```sql
SELECT CONCAT_WS(',','First name','Second name','Last Name');
+-------------------------------------------------------+
| CONCAT_WS(',','First name','Second name','Last Name') |
+-------------------------------------------------------+
| First name,Second name,Last Name                      |
+-------------------------------------------------------+

SELECT CONCAT_WS('-','Floor',NULL,'Room');
+------------------------------------+
| CONCAT_WS('-','Floor',NULL,'Room') |
+------------------------------------+
| Floor-Room                         |
+------------------------------------+
```

In some cases, remember to include a space in the separator string:

```sql
SET @a = 'gnu', @b = 'penguin', @c = 'sea lion';
Query OK, 0 rows affected (0.00 sec)

SELECT CONCAT_WS(', ', @a, @b, @c);
+-----------------------------+
| CONCAT_WS(', ', @a, @b, @c) |
+-----------------------------+
| gnu, penguin, sea lion      |
+-----------------------------+
```

Using `CONCAT_WS()` to handle `NULL`s:

```sql
SET @a = 'a', @b = NULL, @c = 'c';

SELECT CONCAT_WS('', @a, @b, @c);
+---------------------------+
| CONCAT_WS('', @a, @b, @c) |
+---------------------------+
| ac                        |
+---------------------------+
```

## See Also

- [GROUP_CONCAT()](/built-in-functions/aggregate-functions/group_concat)