# QUOTE

## Syntax

```sql
QUOTE(str)
```

## Description

Quotes a string to produce a result that can be used as a properly escaped data
value in an SQL statement. The string is returned enclosed by single quotes and
with each instance of single quote ("`'`"), backslash ("`\`"),
`ASCII NUL`, and Control-Z preceded by a backslash. If the argument
is `NULL`, the return value is the word "`NULL`" without enclosing single
quotes.

## Examples

```sql
SELECT QUOTE("Don't!");
+-----------------+
| QUOTE("Don't!") |
+-----------------+
| 'Don\'t!'       |
+-----------------+

SELECT QUOTE(NULL); 
+-------------+
| QUOTE(NULL) |
+-------------+
| NULL        |
+-------------+
```