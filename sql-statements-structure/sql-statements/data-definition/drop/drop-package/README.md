# DROP PACKAGE

##### MariaDB starting with [10.3.5](/kb/en/mariadb-1035-release-notes/)

Oracle-style packages were introduced in [MariaDB 10.3.5](/kb/en/mariadb-1035-release-notes/).

## Syntax

```sql
DROP PACKAGE [IF EXISTS]  [ db_name . ] package_name
```

## Description

The `DROP PACKAGE` statement can be used when [Oracle SQL_MODE](/kb/en/sql_modeoracle-from-mariadb-103/) is set.

The `DROP PACKAGE` statement drops a stored package entirely:

- Drops the package specification (earlier created using the [CREATE PACKAGE](/sql-statements-structure/sql-statements/data-definition/create/create-package) statement).
- Drops the package implementation, if the implementation was already created using the [CREATE PACKAGE BODY](/sql-statements-structure/sql-statements/data-definition/create/create-package-body) statement.

## See Also

- [SHOW CREATE PACKAGE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-package)
- [CREATE PACKAGE](/sql-statements-structure/sql-statements/data-definition/create/create-package)
- [CREATE PACKAGE BODY](/sql-statements-structure/sql-statements/data-definition/create/create-package-body)
- [DROP PACKAGE BODY](/sql-statements-structure/sql-statements/data-definition/drop/drop-package-body)
- [Oracle SQL_MODE](/kb/en/sql_modeoracle-from-mariadb-103/)