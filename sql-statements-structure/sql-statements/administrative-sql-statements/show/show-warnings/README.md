# SHOW WARNINGS

## Syntax

```sql
SHOW WARNINGS [LIMIT [offset,] row_count]
SHOW ERRORS [LIMIT row_count OFFSET offset]
SHOW COUNT(*) WARNINGS
```

## Description

<code class="highlight fixed" style="white-space:pre-wrap">SHOW WARNINGS</code> shows the error, warning, and note messages
that resulted from the last statement that generated messages in the
current session.  It shows nothing if the last statement used a table
and generated no messages. (That is, a statement that uses a table but
generates no messages clears the message list.) Statements that do not
use tables and do not generate messages have no effect on the message
list.

A note is different to a warning in that it only appears if the <a undefined>sql_notes</a> variable is set to 1 (the default), and is not converted to an error if <a undefined>strict mode</a> is enabled.

A related statement, <code class="highlight fixed" style="white-space:pre-wrap">[SHOW ERRORS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-errors/)</code>, shows only the errors.

The <code class="highlight fixed" style="white-space:pre-wrap">SHOW COUNT(*) WARNINGS</code> statement displays the total
number of errors, warnings, and notes. You can also retrieve this number from
the [warning_count](/kb/en/server-system-variables/#warning_count) variable:

```sql
SHOW COUNT(*) WARNINGS;
SELECT @@warning_count;
```

The value of <a undefined>warning_count</a> might be greater than the number of messages displayed by <code class="highlight fixed" style="white-space:pre-wrap">SHOW WARNINGS</code> if the <a undefined>max_error_count</a> system variable is set so low that not all messages are stored.

The <code class="highlight fixed" style="white-space:pre-wrap">LIMIT</code> clause has the same syntax as for the
 <code class="highlight fixed" style="white-space:pre-wrap">[SELECT statement](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/)</code>.

<code class="highlight fixed" style="white-space:pre-wrap">SHOW WARNINGS</code> can be used after [EXPLAIN EXTENDED](/sql-statements-structure/sql-statements/administrative-sql-statements/analyze-and-explain-statements/explain/) to see how a query is internally rewritten by MariaDB.

If the <a undefined>sql_notes</a> server variable is set to 1, Notes are included in the output of `SHOW WARNINGS`; if it is set to 0, this statement will not show (or count) Notes.

The results of `SHOW WARNINGS` and `SHOW COUNT(*) WARNINGS` are directly sent to the client. If you need to access those information in a stored program, you can use the [GET DIAGNOSTICS](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-diagnostics/get-diagnostics/) statement instead.

For a list of MariaDB error codes, see [MariaDB Error Codes](/sql-statements-structure/sql-language-structure/mariadb-error-codes/).

The [mysql](/clients-utilities/mysql-client/mysql-command-line-client/) client also has a number of options related to warnings.  The `\W` command will show warnings after every statement, while `\w` will disable this. Starting the client with the `--show-warnings` option will show warnings after every statement.

[MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/) implements a stored routine error stack trace. `SHOW WARNINGS` can also be used to show more information. See the example below.

## Examples

```sql
SELECT 1/0;
+------+
| 1/0  |
+------+
| NULL |
+------+

SHOW COUNT(*) WARNINGS;
+-------------------------+
| @@session.warning_count |
+-------------------------+
|                       1 |
+-------------------------+

SHOW WARNINGS;
+---------+------+---------------+
| Level   | Code | Message       |
+---------+------+---------------+
| Warning | 1365 | Division by 0 |
+---------+------+---------------+
```

### Stack Trace

From [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/), displaying a stack trace:

```sql
DELIMITER $$
CREATE OR REPLACE PROCEDURE p1()
  BEGIN
    DECLARE c CURSOR FOR SELECT * FROM not_existing;
    OPEN c;
    CLOSE c;
  END;
$$
CREATE OR REPLACE PROCEDURE p2()
  BEGIN
    CALL p1;
  END;
$$
DELIMITER ;
CALL p2;
ERROR 1146 (42S02): Table 'test.not_existing' doesn't exist

SHOW WARNINGS;
+-------+------+-----------------------------------------+
| Level | Code | Message                                 |
+-------+------+-----------------------------------------+
| Error | 1146 | Table 'test.not_existing' doesn't exist |
| Note  | 4091 | At line 6 in test.p1                    |
| Note  | 4091 | At line 4 in test.p2                    |
+-------+------+-----------------------------------------+
```

`SHOW WARNINGS` displays a stack trace, showing where the error actually happened:

- Line 4 in test.p1 is the OPEN command which actually raised the error
- Line 3 in test.p2 is the CALL statement, calling p1 from p2.

## See Also

- [SHOW ERRORS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-errors/)