# SHOW ERRORS

## Syntax

```sql
SHOW ERRORS [LIMIT [offset,] row_count]
SHOW ERRORS [LIMIT row_count OFFSET offset]
SHOW COUNT(*) ERRORS
```

## Description

This statement is similar to [SHOW WARNINGS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-warnings/), except that instead of
displaying errors, warnings, and notes, it displays only errors.

The <code class="highlight fixed" style="white-space:pre-wrap">LIMIT</code> clause has the same syntax as for the
[SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/) statement.

The <code class="highlight fixed" style="white-space:pre-wrap">SHOW COUNT(*) ERRORS</code> statement displays the number of
errors. You can also retrieve this number from the [error_count](/kb/en/server-system-variables/#error_count) variable.

```sql
SHOW COUNT(*) ERRORS;
SELECT @@error_count;
```

The value of [error_count](/kb/en/server-system-variables/#error_count) might be greater than the number of messages displayed by [SHOW WARNINGS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-warnings/) if the [max_error_count](/kb/en/server-system-variables/#max_error_count) system variable is set so low that not all messages are stored.

For a list of MariaDB error codes, see [MariaDB Error Codes](/sql-statements-structure/sql-language-structure/mariadb-error-codes/).

## Examples

```sql
SELECT f();
ERROR 1305 (42000): FUNCTION f does not exist

SHOW COUNT(*) ERRORS;
+-----------------------+
| @@session.error_count |
+-----------------------+
|                     1 |
+-----------------------+

SHOW ERRORS;
+-------+------+---------------------------+
| Level | Code | Message                   |
+-------+------+---------------------------+
| Error | 1305 | FUNCTION f does not exist |
+-------+------+---------------------------+
```