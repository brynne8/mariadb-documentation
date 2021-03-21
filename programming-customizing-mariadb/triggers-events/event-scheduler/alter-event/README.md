# ALTER EVENT

Modifies one or more characteristics of an existing event.

## Syntax

```sql
ALTER
    [DEFINER = { user | CURRENT_USER }]
    EVENT event_name
    [ON SCHEDULE schedule]
    [ON COMPLETION [NOT] PRESERVE]
    [RENAME TO new_event_name]
    [ENABLE | DISABLE | DISABLE ON SLAVE]
    [COMMENT 'comment']
    [DO sql_statement]
```

## Description

The `ALTER EVENT` statement is used to change one or more of the
characteristics of an existing [event](/programming-customizing-mariadb/triggers-events/event-scheduler/events) without the need to drop and recreate it.
The syntax for each of the `DEFINER`, `ON SCHEDULE`, `ON COMPLETION`,
`COMMENT`, `ENABLE` `/` `DISABLE`, and `DO` clauses is exactly the
same as when used with [CREATE EVENT](/sql-statements-structure/sql-statements/data-definition/create/create-event).

This statement requires the <a undefined>EVENT</a> privilege.
When a user executes a successful `ALTER EVENT` statement, that user becomes
the definer for the affected event.

(In MySQL 5.1.11 and earlier, an event could be altered only by its definer, or
by a user having the <a undefined>SUPER</a> privilege.)

`ALTER EVENT` works only with an existing event:

```sql
ALTER EVENT no_such_event ON SCHEDULE EVERY '2:3' DAY_HOUR;
ERROR 1539 (HY000): Unknown event 'no_such_event'
```

## Examples

```sql
ALTER EVENT myevent 
  ON SCHEDULE AT CURRENT_TIMESTAMP + INTERVAL 2 HOUR 
  DO 
    UPDATE myschema.mytable SET mycol = mycol + 1;
```

## See Also

- [Events Overview](/kb/en/events-overview/)
- [CREATE EVENT](/sql-statements-structure/sql-statements/data-definition/create/create-event)
- [SHOW CREATE EVENT](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-event)
- [DROP EVENT](/sql-statements-structure/sql-statements/data-definition/drop/drop-event)