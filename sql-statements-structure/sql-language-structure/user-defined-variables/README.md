# User-Defined Variables

User-defined variables are variables which can be created by the user and exist in the session. This means that no one can access user-defined variables that have been set by another user, and when the session is closed these variables expire. However, these variables can be shared between several queries and [stored programs](/kb/en/stored-programs-and-views/).

User-defined variables names must be preceded by a single <em>at</em> character (`@`). While it is safe to use a reserved word as a user-variable name, the only allowed characters are ASCII letters, digits, dollar sign (`$`), underscore (`_`) and dot (`.`). If other characters are used, the name can be quoted in one of the following ways:

- @`var_name`
- @'var_name'
- @"var_name"

These characters can be escaped as usual.

User-variables names are case insensitive, though they were case sensitive in MySQL 4.1 and older versions.

User-defined variables cannot be declared. They can be read even if no value has been set yet; in that case, they are NULL. To set a value for a user-defined variable you can use:

- [SET](/sql-statements-structure/sql-statements/administrative-sql-statements/set-commands/set) statement;
- [:=](/kb/en/assignment-operator/) operator within a SQL statement;
- [SELECT ... INTO](/kb/en/select-into/).

Since user-defined variables type cannot be declared, the only way to force their type is using [CAST()](/built-in-functions/string-functions/cast) or [CONVERT()](/built-in-functions/string-functions/convert):

SET @str = CAST(123 AS CHAR(5));

If a variable has not been used yet, its value is NULL:

```sql
SELECT @x IS NULL;
+------------+
| @x IS NULL |
+------------+
|          1 |
+------------+
```

It is unsafe to read a user-defined variable and set its value in the same statement (unless the command is SET), because the order of these actions is undefined.

User-defined variables can be used in most MariaDB's statements and clauses which accept an SQL expression. However there are some exceptions, like the [LIMIT](/kb/en/select/#limit) clause.

They must be used to [PREPARE](/sql-statements-structure/sql-statements/prepared-statements/prepare-statement) a prepared statement:

```sql
@sql = 'DELETE FROM my_table WHERE c>1;';
PREPARE stmt FROM @sql;
EXECUTE stmt;
DEALLOCATE PREPARE stmt;
```

Another common use is to include a counter in a query:

```sql
SET @var = 0;
SELECT a, b, c, (@var:=@var+1) AS counter FROM my_table;
```

## See Also

- [DECLARE VARIABLE](/programming-customizing-mariadb/programmatic-compound-statements/declare-variable)
- [Information Schema USER_VARIABLES Table](/kb/en/information-schema-user_variables-table/)