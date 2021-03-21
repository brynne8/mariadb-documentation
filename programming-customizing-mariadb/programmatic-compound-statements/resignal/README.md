# RESIGNAL

## Syntax

```sql
RESIGNAL [error_condition]
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

## Description

The syntax of `RESIGNAL` and its semantics are very similar to [SIGNAL](/programming-customizing-mariadb/programmatic-compound-statements/signal/). This statement can only be used within an error [HANDLER](/programming-customizing-mariadb/programmatic-compound-statements/declare-handler/). It produces an error, like [SIGNAL](/programming-customizing-mariadb/programmatic-compound-statements/signal/). `RESIGNAL` clauses are the same as SIGNAL, except that they all are optional, even [SQLSTATE](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-diagnostics/sqlstate/). All the properties which are not specified in `RESIGNAL`, will be identical to the properties of the error that was received by the error [HANDLER](/sql-statements-structure/nosql/handler/). For a description of the clauses, see [diagnostics area](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-diagnostics/diagnostics-area/).

Note that `RESIGNAL` does not empty the diagnostics area: it just appends another error condition.

`RESIGNAL`, without any clauses, produces an error which is identical to the error that was received by [HANDLER](/sql-statements-structure/nosql/handler/).

If used out of a [HANDLER](/sql-statements-structure/nosql/handler/) construct, RESIGNAL produces the following error:

```sql
ERROR 1645 (0K000): RESIGNAL when handler not active
```

In [MariaDB 5.5](/kb/en/what-is-mariadb-55/), if a [HANDLER](/sql-statements-structure/nosql/handler/) contained a [CALL](/sql-statements-structure/sql-statements/stored-routine-statements/call/) to another procedure, that procedure could use `RESIGNAL`. Since [MariaDB 10.0](/kb/en/what-is-mariadb-100/), trying to do this raises the above error.

For a list of `SQLSTATE` values and MariaDB error codes, see [MariaDB Error Codes](/sql-statements-structure/sql-language-structure/mariadb-error-codes/).

The following procedure tries to query two tables which don't exist, producing a 1146 error in both cases. Those errors will trigger the [HANDLER](/sql-statements-structure/nosql/handler/). The first time the error will be ignored and the client will not receive it, but the second time, the error is re-signaled, so the client will receive it.

```sql
CREATE PROCEDURE test_error( )
BEGIN
   DECLARE CONTINUE HANDLER
      FOR 1146
   BEGIN
   IF @hide_errors IS FALSE THEN
      RESIGNAL;
   END IF;
   END;
   SET @hide_errors = TRUE;
   SELECT 'Next error will be ignored' AS msg;
   SELECT `c` FROM `temptab_one`;
   SELECT 'Next error won''t be ignored' AS msg;
   SET @hide_errors = FALSE;
   SELECT `c` FROM `temptab_two`;
END;

CALL test_error( );

+----------------------------+
| msg                        |
+----------------------------+
| Next error will be ignored |
+----------------------------+

+-----------------------------+
| msg                         |
+-----------------------------+
| Next error won't be ignored |
+-----------------------------+

ERROR 1146 (42S02): Table 'test.temptab_two' doesn't exist
```

The following procedure re-signals an error, modifying only the error message to clarify the cause of the problem.

```sql
CREATE PROCEDURE test_error()
BEGIN
   DECLARE CONTINUE HANDLER
   FOR 1146
   BEGIN
      RESIGNAL SET
      MESSAGE_TEXT = '`temptab` does not exist';
   END;
   SELECT `c` FROM `temptab`;
END;

CALL test_error( );
ERROR 1146 (42S02): `temptab` does not exist
```

As explained above, this works on [MariaDB 5.5](/kb/en/what-is-mariadb-55/), but produces a 1645 error since 10.0.

```sql
CREATE PROCEDURE handle_error()
BEGIN
  RESIGNAL;
END;
CREATE PROCEDURE p()
BEGIN
  DECLARE EXIT HANDLER FOR SQLEXCEPTION CALL p();
  SIGNAL SQLSTATE '45000';
END;
```

## See Also

- [Diagnostics Area](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-diagnostics/diagnostics-area/)
- [SIGNAL](/programming-customizing-mariadb/programmatic-compound-statements/signal/)
- [HANDLER](/sql-statements-structure/nosql/handler/)
- [Stored Routines](/kb/en/stored-programs-and-views/)
- [MariaDB Error Codes](/sql-statements-structure/sql-language-structure/mariadb-error-codes/)