# REGEXP

## Syntax

```sql
expr REGEXP pat, expr RLIKE pat
```

## Description

Performs a pattern match of a string expression `expr` against a pattern
`pat`. The pattern can be an extended regular expression. See [Regular Expressions Overview](/built-in-functions/string-functions/regular-expressions-functions/regular-expressions-overview/) for details on the syntax for
regular expressions (but see also [PCRE Regular Expressions](/kb/en/pcre-regular-expressions/) for syntax introduced in [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/)).

Returns `1` if `expr` matches `pat` or `0` if it doesn't match. If either `expr` or `pat` are NULL, the result is NULL.

The negative form [NOT REGEXP](/built-in-functions/string-functions/not-regexp/) also exists, as an alias for `NOT (string REGEXP pattern)`. RLIKE and NOT RLIKE are synonyms for REGEXP and NOT REGEXP, originally provided for mSQL compatibility.

The pattern need not be a literal string. For example, it can be
specified as a string expression or table column.

<strong>Note:</strong> Because MariaDB uses the C escape syntax in strings (for
example, "\n" to represent the newline character), you must double any
"\" that you use in your REGEXP strings.

REGEXP is not case sensitive, except when used with binary strings.

[MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/) moved to the PCRE regex library - see [PCRE Regular Expressions](/kb/en/pcre-regular-expressions/) for enhancements to REGEXP introduced in [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/).

[MariaDB 10.0.11](/kb/en/mariadb-10011-release-notes/) introduced the [default_regex_flags](/kb/en/server-system-variables/#default_regex_flags) variable to address the remaining compatibilities between PCRE and the old regex library.

## Examples

```sql
SELECT 'Monty!' REGEXP 'm%y%%';
+-------------------------+
| 'Monty!' REGEXP 'm%y%%' |
+-------------------------+
|                       0 |
+-------------------------+

SELECT 'Monty!' REGEXP '.*';
+----------------------+
| 'Monty!' REGEXP '.*' |
+----------------------+
|                    1 |
+----------------------+

SELECT 'new*\n*line' REGEXP 'new\\*.\\*line';
+---------------------------------------+
| 'new*\n*line' REGEXP 'new\\*.\\*line' |
+---------------------------------------+
|                                     1 |
+---------------------------------------+

SELECT 'a' REGEXP 'A', 'a' REGEXP BINARY 'A';
+----------------+-----------------------+
| 'a' REGEXP 'A' | 'a' REGEXP BINARY 'A' |
+----------------+-----------------------+
|              1 |                     0 |
+----------------+-----------------------+

SELECT 'a' REGEXP '^[a-d]';
+---------------------+
| 'a' REGEXP '^[a-d]' |
+---------------------+
|                   1 |
+---------------------+
```

### default_regex_flags examples

[MariaDB 10.0.11](/kb/en/mariadb-10011-release-notes/) introduced the [default_regex_flags](/kb/en/server-system-variables/#default_regex_flags) variable to address the remaining compatibilities between PCRE and the old regex library.

The default behaviour (multiline match is off)

```sql
SELECT 'a\nb\nc' RLIKE '^b$';
+---------------------------+
| '(?m)a\nb\nc' RLIKE '^b$' |
+---------------------------+
|                         0 |
+---------------------------+
```

Enabling the multiline option using the PCRE option syntax:

```sql
SELECT 'a\nb\nc' RLIKE '(?m)^b$';
+---------------------------+
| 'a\nb\nc' RLIKE '(?m)^b$' |
+---------------------------+
|                         1 |
+---------------------------+
```

Enabling the miltiline option using default_regex_flags

```sql
SET default_regex_flags='MULTILINE';
SELECT 'a\nb\nc' RLIKE '^b$';
+-----------------------+
| 'a\nb\nc' RLIKE '^b$' |
+-----------------------+
|                     1 |
+-----------------------+ 
```