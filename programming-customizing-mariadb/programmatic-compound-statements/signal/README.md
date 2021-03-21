# SIGNAL

## Syntax

```sql
SIGNAL error_condition
    [SET error_property
    [, error_property] ...]

error_condition:
    SQLSTATE [VALUE] 'sqlstate_value'
  | condition_name

error_property:
    error_property_name = <error_property_value>

error_property_name:
    CLASS_ORIGIN
  | SUBCLASS_ORIGIN
  | MESSAGE_TEXT
  | MYSQL_ERRNO
  | CONSTRAINT_CATALOG
  | CONSTRAINT_SCHEMA
  | CONSTRAINT_NAME
  | CATALOG_NAME
  | SCHEMA_NAME
  | TABLE_NAME
  | COLUMN_NAME
  | CURSOR_NAME
```

`SIGNAL` empties the [diagnostics area](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-diagnostics/diagnostics-area) and produces a custom error. This statement can be used anywhere, but is generally useful when used inside a [stored program](/kb/en/stored-programs-and-views/). When the error is produced, it can be caught by a [HANDLER](/sql-statements-structure/nosql/handler). If not, the current stored program, or the current statement, will terminate with the specified error.

Sometimes an error [HANDLER](/sql-statements-structure/nosql/handler) just needs to [SIGNAL](/programming-customizing-mariadb/programmatic-compound-statements/signal) the same error it received, optionally with some changes. Usually the [RESIGNAL](/programming-customizing-mariadb/programmatic-compound-statements/resignal) statement is the most convenient way to do this.

`error_condition` can be an [SQLSTATE](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-diagnostics/sqlstate) value or a named error condition defined via [DECLARE CONDITION](/programming-customizing-mariadb/programmatic-compound-statements/declare-condition). [SQLSTATE](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-diagnostics/sqlstate) must be a constant string consisting of five characters. These codes are standard to ODBC and ANSI SQL. For customized errors, the recommended [SQLSTATE](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-diagnostics/sqlstate) is '45000'. For a list of SQLSTATE values used by MariaDB, see the [MariaDB Error Codes](/sql-statements-structure/sql-language-structure/mariadb-error-codes) page. The [SQLSTATE](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-diagnostics/sqlstate) can be read via the API method `mysql_sqlstate( )`.

To specify error properties user-defined variables and [local variables](/programming-customizing-mariadb/programmatic-compound-statements/declare-variable) can be used, as well as [character set conversions](/kb/en/setting-character-sets-and-collations/#literals) (but you can't set a collation).

The error properties, their type and their default values are explained in the [diagnostics area](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-diagnostics/diagnostics-area) page.

## Errors

If the SQLSTATE is not valid, the following error like this will be produced:

```sql
ERROR 1407 (42000): Bad SQLSTATE: '123456'
```

If a property is specified more than once, an error like this will be produced:

```sql
ERROR 1641 (42000): Duplicate condition information item 'MESSAGE_TEXT'
```

If you specify a condition name which is not declared, an error like this will be produced:

```sql
ERROR 1319 (42000): Undefined CONDITION: cond_name
```

If MYSQL_ERRNO is out of range, you will get an error like this:

```sql
ERROR 1231 (42000): Variable 'MYSQL_ERRNO' can't be set to the value of '0'
```

## Examples

Here's what happens if [SIGNAL](/programming-customizing-mariadb/programmatic-compound-statements/signal) is used in the client to generate errors:

```sql
SIGNAL SQLSTATE '01000';
Query OK, 0 rows affected, 1 warning (0.00 sec)

SHOW WARNINGS;

+---------+------+------------------------------------------+
| Level   | Code | Message                                  |
+---------+------+------------------------------------------+
| Warning | 1642 | Unhandled user-defined warning condition |
+---------+------+------------------------------------------+
1 row in set (0.06 sec)

SIGNAL SQLSTATE '02000';
ERROR 1643 (02000): Unhandled user-defined not found condition
```

How to specify MYSQL_ERRNO and MESSAGE_TEXT properties:

```sql
SIGNAL SQLSTATE '45000' SET MYSQL_ERRNO=30001, MESSAGE_TEXT='H
ello, world!';

ERROR 30001 (45000): Hello, world!
```

The following code shows how to use user variables, local variables and character set conversion with SIGNAL:

```sql
CREATE PROCEDURE test_error(x INT)
BEGIN
   DECLARE errno SMALLINT UNSIGNED DEFAULT 31001;
   SET @errmsg = 'Hello, world!';
   IF x = 1 THEN
      SIGNAL SQLSTATE '45000' SET
      MYSQL_ERRNO = errno,
      MESSAGE_TEXT = @errmsg;
   ELSE
      SIGNAL SQLSTATE '45000' SET
      MYSQL_ERRNO = errno,
      MESSAGE_TEXT = _utf8'Hello, world!';
   END IF;
END;
```

How to use named error conditions:

```sql
CREATE PROCEDURE test_error(n INT)
BEGIN
   DECLARE `too_big` CONDITION FOR SQLSTATE '45000';
   IF n > 10 THEN
      SIGNAL `too_big`;
   END IF;
END;
```

In this example, we'll define a [HANDLER](/sql-statements-structure/nosql/handler) for an error code. When the error occurs, we [SIGNAL](/programming-customizing-mariadb/programmatic-compound-statements/signal) a more informative error which makes sense for our procedure:

```sql
CREATE PROCEDURE test_error()
BEGIN
   DECLARE EXIT HANDLER
   FOR 1146
   BEGIN
      SIGNAL SQLSTATE '45000' SET
      MESSAGE_TEXT = 'Temporary tables not found; did you call init() procedure?';
   END;
   -- this will produce a 1146 error
   SELECT `c` FROM `temptab`;
END;
```

## See Also

- [Diagnostics Area](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-diagnostics/diagnostics-area)
- [RESIGNAL](/programming-customizing-mariadb/programmatic-compound-statements/resignal)
- [HANDLER](/sql-statements-structure/nosql/handler)
- [Stored Routines](/kb/en/stored-programs-and-views/)
- [MariaDB Error Codes](/sql-statements-structure/sql-language-structure/mariadb-error-codes)