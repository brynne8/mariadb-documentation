# SHOW PACKAGE BODY STATUS

##### MariaDB starting with [10.3.5](/kb/en/mariadb-1035-release-notes/)

Oracle-style packages were introduced in [MariaDB 10.3.5](/kb/en/mariadb-1035-release-notes/).

## Syntax

```sql
SHOW PACKAGE BODY STATUS
    [LIKE 'pattern' | WHERE expr]
```

## Description

The `SHOW PACKAGE BODY STATUS` statement returns characteristics of stored package bodies (implementations), such as the database, name, type, creator, creation and modification dates, and character set information. A similar statement, <code class="highlight fixed" style="white-space:pre-wrap">[SHOW PACKAGE STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-package-status)</code>, displays information about stored package specifications.

The <code class="highlight fixed" style="white-space:pre-wrap">LIKE</code> clause, if present, indicates which package names to match. The <code class="highlight fixed" style="white-space:pre-wrap">WHERE</code> and <code class="highlight fixed" style="white-space:pre-wrap">LIKE</code> clauses can be given to select rows using more general conditions, as discussed in [Extended SHOW](/sql-statements-structure/sql-statements/administrative-sql-statements/show/extended-show).

The [ROUTINES table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-routines-table) in the INFORMATION_SCHEMA database contains more detailed information.

## Examples

```sql
SHOW PACKAGE BODY STATUS LIKE 'pkg1'\G
*************************** 1. row ***************************
                  Db: test
                Name: pkg1
                Type: PACKAGE BODY
             Definer: root@localhost
            Modified: 2018-02-27 14:44:14
             Created: 2018-02-27 14:44:14
       Security_type: DEFINER
             Comment: This is my first package body
character_set_client: utf8
collation_connection: utf8_general_ci
  Database Collation: latin1_swedish_ci
```

## See Also

- [SHOW PACKAGE STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-package-status)
- [SHOW CREATE PACKAGE BODY](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-package-body)
- [CREATE PACKAGE BODY](/sql-statements-structure/sql-statements/data-definition/create/create-package-body)
- [DROP PACKAGE BODY](/sql-statements-structure/sql-statements/data-definition/drop/drop-package-body)
- [Oracle SQL_MODE](/kb/en/sql_modeoracle-from-mariadb-103/)