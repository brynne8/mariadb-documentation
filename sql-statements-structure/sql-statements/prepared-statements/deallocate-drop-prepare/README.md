# DEALLOCATE / DROP PREPARE

## Syntax

```sql
{DEALLOCATE | DROP} PREPARE stmt_name
```

## Description

To deallocate a prepared statement produced with [PREPARE](/sql-statements-structure/sql-statements/prepared-statements/prepare-statement), use a
<code class="fixed" style="white-space:pre-wrap">DEALLOCATE PREPARE</code> statement that refers to the prepared statement
name.

A prepared statement is implicitly deallocated when a new `PREPARE` command is issued. In that case, there is no need to use `DEALLOCATE`.

Attempting to execute a prepared statement after deallocating it
results in an error, as if it was not prepared at all:

```sql
ERROR 1243 (HY000): Unknown prepared statement handler (stmt_name) given to EXECUTE
```

If the specified statement has not been PREPAREd, an error similar to the following will be produced:

```sql
ERROR 1243 (HY000): Unknown prepared statement handler (stmt_name) given to DEALLOCATE PREPARE
```

## Example

See [example in PREPARE](/kb/en/prepare-statement/#example).

## See Also

- [PREPARE Statement](/sql-statements-structure/sql-statements/prepared-statements/prepare-statement)
- [EXECUTE Statement](/sql-statements-structure/sql-statements/prepared-statements/execute-statement)
- [EXECUTE IMMEDIATE](/sql-statements-structure/sql-statements/prepared-statements/execute-immediate)