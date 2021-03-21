# SHOW EVENTS

## Syntax

```sql
SHOW EVENTS [{FROM | IN} schema_name]
    [LIKE 'pattern' | WHERE expr]
```

## Description

Shows information about Event Manager [events](/programming-customizing-mariadb/triggers-events/event-scheduler/events) (created with [CREATE EVENT](/sql-statements-structure/sql-statements/data-definition/create/create-event)). Requires the <a undefined>EVENT</a> privilege. Without any arguments, <code class="highlight fixed" style="white-space:pre-wrap">SHOW EVENTS</code> lists all of the events in the current schema:

```sql
SELECT CURRENT_USER(), SCHEMA();
+----------------+----------+
| CURRENT_USER() | SCHEMA() |
+----------------+----------+
| jon@ghidora    | myschema |
+----------------+----------+

SHOW EVENTS\G
*************************** 1. row ***************************
                  Db: myschema
                Name: e_daily
             Definer: jon@ghidora
           Time zone: SYSTEM
                Type: RECURRING
          Execute at: NULL
      Interval value: 10
      Interval field: SECOND
              Starts: 2006-02-09 10:41:23
                Ends: NULL
              Status: ENABLED
          Originator: 0
character_set_client: latin1
collation_connection: latin1_swedish_ci
  Database Collation: latin1_swedish_ci
```

To see the event action, use [SHOW CREATE EVENT](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-event) instead, or look at the [information_schema.EVENTS](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-events-table) table.

To see events for a specific schema, use the <code class="highlight fixed" style="white-space:pre-wrap">FROM</code> clause.
For example, to see events for the test schema, use the following statement:

```sql
SHOW EVENTS FROM test;
```

The <code class="highlight fixed" style="white-space:pre-wrap">LIKE</code> clause, if present, indicates which event names to
match. The <code class="highlight fixed" style="white-space:pre-wrap">WHERE</code> clause can be given to select rows using
more general conditions, as discussed in [Extended Show](/sql-statements-structure/sql-statements/administrative-sql-statements/show/extended-show).