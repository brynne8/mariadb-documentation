# GET DIAGNOSTICS

##### MariaDB starting with [10.0.4](/kb/en/mariadb-1004-release-notes/)

The GET DIAGNOSTICS statement was added in [MariaDB 10.0.4](/kb/en/what-is-mariadb-100/).

```sql
GET [CURRENT] DIAGNOSTICS
{
    statement_property
    [, statement_property] ... 
  | CONDITION condition_number
    condition_property
    [, condition_property] ...
}

statement_property:
    variable = statement_property_name

condition_property:
    variable  = condition_property_name

statement_property_name:
    NUMBER
  | ROW_COUNT

condition_property_name:
    CLASS_ORIGIN
  | SUBCLASS_ORIGIN
  | RETURNED_SQLSTATE
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

The [diagnostics area](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-diagnostics/diagnostics-area/) contains information about the errors, warnings and notes which were produced by the last SQL statement. If that statement didn't produce any warnings, the diagnostics area contains information about the last executed statement which involved a table. The GET DIAGNOSTICS statement copies the requested information from the diagnostics area to the specified variables. It is possible to use both user variables or [local variables](/programming-customizing-mariadb/programmatic-compound-statements/declare-variable/).

To use GET DIAGNOSTICS, it is important to know how the diagnostics area is structured. It has two sub-areas: the statement information area and the error conditions information area. For details, please refer to the [diagnostics area](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-diagnostics/diagnostics-area/) page.

Each single GET DIAGNOSTICS command can read information from the statement information area or from a single error condition. This means that, if you have two warnings and you want to know the number of warnings, and read both the warnings, you need to issue GET DIAGNOSTICS three times.

The CURRENT keywords adds nothing to the statement, because MariaDB has only one diagnostics area.

If GET DIAGNOSTICS produces an error condition (because the command is properly parsed but not correctly used), the diagnostics area is not emptied, and the new condition is added.

## Getting Information from a Condition

To read information from a condition, the CONDITION keyword must be specified and it must be followed by the condition number. This number can be specified as a constant value or as a variable. The first condition's index is 1. If the error condition does not exist, the variables will not change their value and a 1758 error will be produced ("Invalid condition number").

The condition properties that can be read with GET DIAGNOSTICS are the same that can be set with SIGNAL and RESIGNAL statements. They are explained in the [diagnostics area](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-diagnostics/diagnostics-area/) page. However, there is one more property: RETURNED_SQLSTATE, which indicates the condition's [SQLSTATE](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-diagnostics/sqlstate/).

For a list of SQLSTATE values and MariaDB error codes, see [MariaDB Error Codes](/sql-statements-structure/sql-language-structure/mariadb-error-codes/).

The type for all the condition properties is VARCHAR(64), except for MYSQL_ERRNO, whose valid range is 1 to 65534.

## Examples

In the following example, a statement generates two warnings, and GET DIAGNOSTICS is used to get the number of warnings:

```sql
CREATE TABLE `test`.`t` (`c` INT) ENGINE = x;
Query OK, 0 rows affected, 2 warnings (0.19 sec)

GET DIAGNOSTICS @num_conditions = NUMBER;

SELECT @num_conditions;
+-----------------+
| @num_conditions |
+-----------------+
|               2 |
+-----------------+
```

Then, we can see the warnings:

```sql
GET DIAGNOSTICS CONDITION 1 @sqlstate = RETURNED_SQLSTATE,
  @errno = MYSQL_ERRNO, @text = MESSAGE_TEXT;

SELECT @sqlstate, @errno, @text;
+-----------+--------+----------------------------+
| @sqlstate | @errno | @text                      |
+-----------+--------+----------------------------+
| 42000     |   1286 | Unknown storage engine 'x' |
+-----------+--------+----------------------------+

GET DIAGNOSTICS CONDITION 2 @sqlstate = RETURNED_SQLSTATE,
  @errno = MYSQL_ERRNO, @text = MESSAGE_TEXT;

SELECT @sqlstate, @errno, @text;
+-----------+--------+-------------------------------------------+
| @sqlstate | @errno | @text                                     |
+-----------+--------+-------------------------------------------+
| HY000     |   1266 | Using storage engine InnoDB for table 't' |
+-----------+--------+-------------------------------------------+
```

## See Also

- [Diagnostics Area](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-diagnostics/diagnostics-area/)