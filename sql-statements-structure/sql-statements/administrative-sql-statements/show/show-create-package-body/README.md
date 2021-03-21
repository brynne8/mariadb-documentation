# SHOW CREATE PACKAGE BODY

##### MariaDB starting with [10.3.5](/kb/en/mariadb-1035-release-notes/)

Oracle-style packages were introduced in [MariaDB 10.3.5](/kb/en/mariadb-1035-release-notes/).

## Syntax

```sql
SHOW CREATE PACKAGE BODY  [ db_name . ] package_name
```

## Description

The `SHOW CREATE PACKAGE BODY` statement can be used when [Oracle SQL_MODE](/kb/en/sql_modeoracle-from-mariadb-103/) is set.

Shows the `CREATE` statement that creates the given package body (i.e. the implementation).

## Examples

```sql
SHOW CREATE PACKAGE BODY employee_tools\G
*************************** 1. row ***************************
        Package body: employee_tools
            sql_mode: PIPES_AS_CONCAT,ANSI_QUOTES,IGNORE_SPACE,ORACLE,NO_KEY_OPTIONS,NO_TABLE_OPTIONS,NO_FIELD_OPTIONS,NO_AUTO_CREATE_USER
 Create Package Body: CREATE DEFINER="root"@"localhost" PACKAGE BODY "employee_tools" AS
  
  stdRaiseAmount DECIMAL(10,2):=500;
  
  PROCEDURE log (eid INT, ecmnt TEXT) AS
  BEGIN
    INSERT INTO employee_log (id, cmnt) VALUES (eid, ecmnt);
  END;
  
  PROCEDURE hire(ename TEXT, esalary DECIMAL(10,2)) AS
    eid INT;
  BEGIN
    INSERT INTO employee (name, salary) VALUES (ename, esalary);
    eid:= last_insert_id();
    log(eid, 'hire ' || ename);
  END;

  FUNCTION getSalary(eid INT) RETURN DECIMAL(10,2) AS
    nSalary DECIMAL(10,2);
  BEGIN
    SELECT salary INTO nSalary FROM employee WHERE id=eid;
    log(eid, 'getSalary id=' || eid || ' salary=' || nSalary);
    RETURN nSalary;
  END;

  PROCEDURE raiseSalary(eid INT, amount DECIMAL(10,2)) AS
  BEGIN
    UPDATE employee SET salary=salary+amount WHERE id=eid;
    log(eid, 'raiseSalary id=' || eid || ' amount=' || amount);
  END;

  PROCEDURE raiseSalaryStd(eid INT) AS
  BEGIN
    raiseSalary(eid, stdRaiseAmount);
    log(eid, 'raiseSalaryStd id=' || eid);
  END;

BEGIN  
  log(0, 'Session ' || connection_id() || ' ' || current_user || ' started');
END
character_set_client: utf8
collation_connection: utf8_general_ci
  Database Collation: latin1_swedish_ci
```

## See also

- [CREATE PACKAGE](/sql-statements-structure/sql-statements/data-definition/create/create-package/)
- [SHOW CREATE PACKAGE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-package/)
- [DROP PACKAGE](/sql-statements-structure/sql-statements/data-definition/drop/drop-package/)
- [CREATE PACKAGE BODY](/sql-statements-structure/sql-statements/data-definition/create/create-package-body/)
- [DROP PACKAGE BODY](/sql-statements-structure/sql-statements/data-definition/drop/drop-package-body/)
- [Oracle SQL_MODE](/kb/en/sql_modeoracle-from-mariadb-103/)