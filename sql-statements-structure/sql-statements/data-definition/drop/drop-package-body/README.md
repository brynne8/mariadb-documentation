# DROP PACKAGE BODY

##### MariaDB starting with [10.3.5](/kb/en/mariadb-1035-release-notes/)

Oracle-style packages were introduced in [MariaDB 10.3.5](/kb/en/mariadb-1035-release-notes/).

## Syntax

```sql
DROP PACKAGE BODY [IF EXISTS]  [ db_name . ] package_name
```

## Description

The `DROP PACKAGE BODY` statement can be used when [Oracle SQL_MODE](/kb/en/sql_modeoracle-from-mariadb-103/) is set.

The `DROP PACKAGE BODY` statement drops the package body (i.e the implementation), previously created using the [CREATE PACKAGE BODY](/sql-statements-structure/sql-statements/data-definition/create/create-package-body) statement.

Note, `DROP PACKAGE BODY` drops only the package implementation, but does not drop the package specification. Use [DROP PACKAGE](/sql-statements-structure/sql-statements/data-definition/drop/drop-package) to drop the package entirely (i.e. both implementation and specification).

## See also

- [CREATE PACKAGE](/sql-statements-structure/sql-statements/data-definition/create/create-package)
- [SHOW CREATE PACKAGE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-package)
- [DROP PACKAGE](/sql-statements-structure/sql-statements/data-definition/drop/drop-package)
- [CREATE PACKAGE BODY](/sql-statements-structure/sql-statements/data-definition/create/create-package-body)
- [SHOW CREATE PACKAGE BODY](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-package-body)
- [Oracle SQL_MODE](/kb/en/sql_modeoracle-from-mariadb-103/)