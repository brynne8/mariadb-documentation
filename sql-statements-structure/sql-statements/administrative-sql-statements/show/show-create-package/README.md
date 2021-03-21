# SHOW CREATE PACKAGE

##### MariaDB starting with [10.3.5](/kb/en/mariadb-1035-release-notes/)

Oracle-style packages were introduced in [MariaDB 10.3.5](/kb/en/mariadb-1035-release-notes/).

## Syntax

```sql
SHOW CREATE PACKAGE  [ db_name . ] package_name
```

## Description

The `SHOW CREATE PACKAGE` statement can be used when [Oracle SQL_MODE](/kb/en/sql_modeoracle-from-mariadb-103/) is set.

Shows the `CREATE` statement that creates the given package specification.

## Examples

```sql
SHOW CREATE PACKAGE employee_tools\G
*************************** 1. row ***************************
             Package: employee_tools
            sql_mode: PIPES_AS_CONCAT,ANSI_QUOTES,IGNORE_SPACE,ORACLE,NO_KEY_OPTIONS,NO_TABLE_OPTIONS,NO_FIELD_OPTIONS,NO_AUTO_CREATE_USER
      Create Package: CREATE DEFINER="root"@"localhost" PACKAGE "employee_tools" AS
  FUNCTION getSalary(eid INT) RETURN DECIMAL(10,2);
  PROCEDURE raiseSalary(eid INT, amount DECIMAL(10,2));
  PROCEDURE raiseSalaryStd(eid INT);
  PROCEDURE hire(ename TEXT, esalary DECIMAL(10,2));
END
character_set_client: utf8
collation_connection: utf8_general_ci
  Database Collation: latin1_swedish_ci
```

## See Also

- [CREATE PACKAGE](/sql-statements-structure/sql-statements/data-definition/create/create-package)
- [DROP PACKAGE](/sql-statements-structure/sql-statements/data-definition/drop/drop-package)
- [CREATE PACKAGE BODY](/sql-statements-structure/sql-statements/data-definition/create/create-package-body)
- [SHOW CREATE PACKAGE BODY](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-package-body)
- [DROP PACKAGE BODY](/sql-statements-structure/sql-statements/data-definition/drop/drop-package-body)
- [Oracle SQL_MODE](/kb/en/sql_modeoracle-from-mariadb-103/)