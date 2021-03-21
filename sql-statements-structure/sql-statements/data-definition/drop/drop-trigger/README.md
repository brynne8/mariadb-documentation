# DROP TRIGGER

## Syntax

```sql
DROP TRIGGER [IF EXISTS] [schema_name.]trigger_name
```

## Description

This statement drops a [trigger](/programming-customizing-mariadb/triggers-events/triggers). The schema (database) name is optional. If the
schema is omitted, the trigger is dropped from the default schema.
Its use requires the `TRIGGER` privilege for the table associated with the trigger.

Use <code class="highlight fixed" style="white-space:pre-wrap">IF EXISTS</code> to prevent an error from occurring for a
trigger that does not exist. A `NOTE` is generated for a non-existent trigger
when using `IF EXISTS`. See [SHOW WARNINGS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-warnings).

<strong>Note:</strong> Triggers for a table are also dropped if you drop the table.

## Examples

```sql
DROP TRIGGER test.example_trigger;
```

Using the IF EXISTS clause:

```sql
DROP TRIGGER IF EXISTS test.example_trigger;
Query OK, 0 rows affected, 1 warning (0.01 sec)

SHOW WARNINGS;
+-------+------+------------------------+
| Level | Code | Message                |
+-------+------+------------------------+
| Note  | 1360 | Trigger does not exist |
+-------+------+------------------------+
```

## See Also

- [Trigger Overview](/programming-customizing-mariadb/triggers-events/triggers/trigger-overview)
- [CREATE TRIGGER](/programming-customizing-mariadb/triggers-events/triggers/create-trigger)
- [Information Schema TRIGGERS Table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-triggers-table)
- [SHOW TRIGGERS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-triggers)
- [SHOW CREATE TRIGGER](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-trigger)
- [Trigger Limitations](/programming-customizing-mariadb/triggers-events/triggers/trigger-limitations)