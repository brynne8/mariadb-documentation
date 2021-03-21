# EXECUTE Statement

## Syntax

```sql
EXECUTE stmt_name
    [USING expression[, expression] ...]
```

##### MariaDB starting with [10.2.3](/kb/en/mariadb-1023-release-notes/)

`EXECUTE` with expression as parameters was introduced in [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/). Before that one could only use variables (@var_name) as parameters.

## Description

After preparing a statement with [PREPARE](/sql-statements-structure/sql-statements/prepared-statements/prepare-statement/), you execute it with an
<code class="fixed" style="white-space:pre-wrap">EXECUTE</code> statement that refers to the prepared statement name. If the
prepared statement contains any parameter markers, you must supply a
<code class="fixed" style="white-space:pre-wrap">USING</code> clause that lists user variables containing the values to be
bound to the parameters. Parameter values can be supplied only by user
variables, and the <code class="fixed" style="white-space:pre-wrap">USING</code> clause must name exactly as many variables as
the number of parameter markers in the statement.

You can execute a given prepared statement multiple times, passing
different variables to it or setting the variables to different values
before each execution.

If the specified statement has not been PREPAREd, an error similar to the following is produced:

```sql
ERROR 1243 (HY000): Unknown prepared statement handler (stmt_name) given to EXECUTE
```

## Example

See [example in PREPARE](/kb/en/prepare-statement/#example).

## See Also

- [EXECUTE IMMEDIATE](/sql-statements-structure/sql-statements/prepared-statements/execute-immediate/)