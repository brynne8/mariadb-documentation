# SHOW CREATE TRIGGER

## Syntax

```sql
SHOW CREATE TRIGGER trigger_name
```

## Description

This statement shows a <code class="highlight fixed" style="white-space:pre-wrap">[CREATE TRIGGER](/programming-customizing-mariadb/triggers-events/triggers/create-trigger)</code>
statement that creates the given trigger, as well as the [SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode) that was used when the trigger has been created and the character set used by the connection.

The output of this statement is unreliably affected by the <a undefined>sql_quote_show_create</a> server system variable - see [http://bugs.mysql.com/bug.php?id=12719](http://bugs.mysql.com/bug.php?id=12719)

## Examples

```sql
SHOW CREATE TRIGGER example\G
*************************** 1. row ***************************
               Trigger: example
              sql_mode: ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,STRICT_ALL_TABLES
,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_
ENGINE_SUBSTITUTION
SQL Original Statement: CREATE DEFINER=`root`@`localhost` TRIGGER example BEFORE
 INSERT ON t FOR EACH ROW
BEGIN
        SET NEW.c = NEW.c * 2;
END
  character_set_client: cp850
  collation_connection: cp850_general_ci
  Database Collation: utf8_general_ci
  Created: 2016-09-29 13:53:34.35
```

##### MariaDB starting with [10.2.3](/kb/en/mariadb-1023-release-notes/)

The `Created` column was added in MySQL 5.7 and [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/) as part of introducing multiple trigger events per action.

## See also

- [Trigger Overview](/programming-customizing-mariadb/triggers-events/triggers/trigger-overview)
- [CREATE TRIGGER](/programming-customizing-mariadb/triggers-events/triggers/create-trigger)
- [DROP TRIGGER](/sql-statements-structure/sql-statements/data-definition/drop/drop-trigger)
- [information_schema.TRIGGERS Table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-triggers-table)
- [SHOW TRIGGERS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-triggers)
- [Trigger Limitations](/programming-customizing-mariadb/triggers-events/triggers/trigger-limitations)