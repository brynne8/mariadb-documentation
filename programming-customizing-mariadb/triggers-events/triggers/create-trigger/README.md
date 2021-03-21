# CREATE TRIGGER

## Syntax

```sql
CREATE [OR REPLACE]
    [DEFINER = { user | CURRENT_USER | role | CURRENT_ROLE }]
    TRIGGER [IF NOT EXISTS] trigger_name trigger_time trigger_event
    ON tbl_name FOR EACH ROW
   [{ FOLLOWS | PRECEDES } other_trigger_name ]
   trigger_stmt;
```

## Description

This statement creates a new [trigger](/programming-customizing-mariadb/triggers-events/triggers). A trigger is a named database
object that is associated with a table, and that activates when a
particular event occurs for the table. The trigger becomes associated
with the table named `tbl_name`, which must refer to a permanent table.
You cannot associate a trigger with a `TEMPORARY` table or a view.

`CREATE TRIGGER` requires the [TRIGGER](/kb/en/grant/#table-privileges) privilege for the table associated
with the trigger.

##### MariaDB starting with [10.2.3](/kb/en/mariadb-1023-release-notes/)

You can have multiple triggers for the same `trigger_time` and `trigger_event`.

For valid identifiers to use as trigger names, see [Identifier Names](/sql-statements-structure/sql-language-structure/identifier-names).

### OR REPLACE

##### MariaDB starting with [10.1.4](/kb/en/mariadb-1014-release-notes/)

If used and the trigger already exists, instead of an error being returned, the existing trigger will be dropped and replaced by the newly defined trigger.

### DEFINER

The `DEFINER` clause determines the security context to be used when
checking access privileges at trigger activation time. Usage requires the [SUPER](/kb/en/grant/#super) privilege, or, from [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), the [SET USER](/kb/en/grant/#set-user) privilege.

### IF NOT EXISTS

##### MariaDB starting with [10.1.4](/kb/en/mariadb-1014-release-notes/)

If the `IF NOT EXISTS` clause is used, the trigger will only be created if a trigger of the same name does not exist. If the trigger already exists, by default a warning will be returned.

### trigger_time

`trigger_time` is the trigger action time. It can be `BEFORE` or `AFTER` to
indicate that the trigger activates before or after each row to be
modified.

### trigger_event

`trigger_event` indicates the kind of statement that activates the
trigger. The `trigger_event` can be one of the following:

- `INSERT`: The trigger is activated whenever a new row is inserted into the table; for example, through [INSERT](/kb/en/insert-commands/), [LOAD DATA](/kb/en/load-data-infile/), and [REPLACE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/replace) statements.
- `UPDATE`: The trigger is activated whenever a row is modified; for example, through [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update) statements.
- `DELETE`: The trigger is activated whenever a row is deleted from the table; for example, through [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete) and [REPLACE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/replace) statements. However, `DROP TABLE` and `TRUNCATE` statements on the table do not activate this trigger, because they do not use `DELETE`. Dropping a partition does not activate `DELETE` triggers, either.

#### FOLLOWS/PRECEDES other_trigger_name

##### MariaDB starting with [10.2.3](/kb/en/mariadb-1023-release-notes/)

The `FOLLOWS other_trigger_name` and `PRECEDES other_trigger_name` options were added in [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/) as part of supporting multiple triggers per action time.
This is the same syntax used by MySQL 5.7, although MySQL 5.7 does not have multi-trigger support.

`FOLLOWS` adds the new trigger after another trigger while `PRECEDES` adds the new trigger before another trigger.  If neither option is used, the new trigger is added last for the given action and time.

`FOLLOWS` and `PRECEDES` are not stored in the trigger definition. However the trigger order is guaranteed to not change over time. [mariadb-dump/mysqldump](/clients-utilities/backup-restore-and-import-clients/mysqldump) and other backup methods will not change trigger order.
You can verify the trigger order from the `ACTION_ORDER` column in [INFORMATION_SCHEMA.TRIGGERS](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-triggers-table) table.

```sql
SELECT trigger_name, action_order FROM information_schema.triggers 
  WHERE event_object_table='t1';
```

## Examples

```sql
CREATE DEFINER=`root`@`localhost` TRIGGER increment_animal
  AFTER INSERT ON animals FOR EACH ROW 
   UPDATE animal_count SET animal_count.animals = animal_count.animals+1;
```

OR REPLACE and IF NOT EXISTS

```sql
CREATE DEFINER=`root`@`localhost` TRIGGER increment_animal
  AFTER INSERT ON animals FOR EACH ROW
    UPDATE animal_count SET animal_count.animals = animal_count.animals+1;
ERROR 1359 (HY000): Trigger already exists

CREATE OR REPLACE DEFINER=`root`@`localhost` TRIGGER increment_animal
  AFTER INSERT ON animals  FOR EACH ROW
    UPDATE animal_count SET animal_count.animals = animal_count.animals+1;
Query OK, 0 rows affected (0.12 sec)

CREATE DEFINER=`root`@`localhost` TRIGGER IF NOT EXISTS increment_animal
  AFTER INSERT ON animals FOR EACH ROW
    UPDATE animal_count SET animal_count.animals = animal_count.animals+1;
Query OK, 0 rows affected, 1 warning (0.00 sec)

SHOW WARNINGS;
+-------+------+------------------------+
| Level | Code | Message                |
+-------+------+------------------------+
| Note  | 1359 | Trigger already exists |
+-------+------+------------------------+
1 row in set (0.00 sec)
```

## See Also

- [Identifier Names](/sql-statements-structure/sql-language-structure/identifier-names)
- [Trigger Overview](/programming-customizing-mariadb/triggers-events/triggers/trigger-overview)
- [DROP TRIGGER](/sql-statements-structure/sql-statements/data-definition/drop/drop-trigger)
- [Information Schema TRIGGERS Table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-triggers-table)
- [SHOW TRIGGERS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-triggers)
- [SHOW CREATE TRIGGER](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-trigger)
- [Trigger Limitations](/programming-customizing-mariadb/triggers-events/triggers/trigger-limitations)