# DROP EVENT

## Syntax

```sql
DROP EVENT [IF EXISTS] event_name
```

## Description

This statement drops the [event](/programming-customizing-mariadb/triggers-events/event-scheduler/events) named `event_name`. The event immediately
ceases being active, and is deleted completely from the server.

If the event does not exist, the error
<code class="fixed" style="white-space:pre-wrap">ERROR 1517 (HY000): Unknown event 'event_name'</code>
results. You can override this and cause the
statement to generate a `NOTE` for non-existent events instead by using
`IF EXISTS`. See [SHOW WARNINGS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-warnings).

This statement requires the <a undefined>EVENT</a> privilege. In MySQL 5.1.11 and earlier, an event could be dropped only
by its definer, or by a user having the <a undefined>SUPER</a> privilege.

## Examples

```sql
DROP EVENT myevent3;
```

Using the IF EXISTS clause:

```sql
DROP EVENT IF EXISTS myevent3;
Query OK, 0 rows affected, 1 warning (0.01 sec)

SHOW WARNINGS;
+-------+------+-------------------------------+
| Level | Code | Message                       |
+-------+------+-------------------------------+
| Note  | 1305 | Event myevent3 does not exist |
+-------+------+-------------------------------+
```

## See also

- [Events Overview](/kb/en/events-overview/)
- [CREATE EVENT](/sql-statements-structure/sql-statements/data-definition/create/create-event)
- [SHOW CREATE EVENT](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-event)
- [ALTER EVENT](/programming-customizing-mariadb/triggers-events/event-scheduler/alter-event)