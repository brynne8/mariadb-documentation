# Events Overview

[Events](stored-programs-and-views-event) are named database objects containing SQL statements that are to be executed at a later stage, either once off, or at regular intervals.

They function very similarly to the Windows Task Scheduler or Unix cron jobs.

Creating, modifying or deleting events requires the [EVENT privilege](/kb/en/grant/#database-privileges).

## Creating events

Events are created with the [CREATE EVENT](/sql-statements-structure/sql-statements/data-definition/create/create-event/) statement.

### Example

```sql
CREATE EVENT test_event 
  ON SCHEDULE EVERY 1 MINUTE DO 
   UPDATE test.t1 SET a = a + 1;
```

## Executing events

Events are only executed if the event scheduler is running. This is determined by the value of the [event_scheduler](/kb/en/server-system-variables/#event_scheduler) system variable, which needs to be set to `On` for the event scheduler to be running.

You can check if the Event scheduler is running with:

```sql
SHOW PROCESSLIST;
+----+-----------------+-----------+------+---------+------+-----------------------------+------------------+----------+
| Id | User            | Host      | db   | Command | Time | State                       | Info             | Progress |
+----+-----------------+-----------+------+---------+------+-----------------------------+------------------+----------+
| 40 | root            | localhost | test | Sleep   | 4687 |                             | NULL             |    0.000 |
| 41 | root            | localhost | test | Query   |    0 | init                        | SHOW PROCESSLIST |    0.000 |
| 42 | event_scheduler | localhost | NULL | Daemon  |   30 | Waiting for next activation | NULL             |    0.000 |
+----+-----------------+-----------+------+---------+------+-----------------------------+------------------+----------+
```

If the event scheduler is not running and `event_scheduler` has been set to `OFF`, use:

```sql
SET GLOBAL event_scheduler = ON;
```

to activate it. If `event_scheduler` has been set to `Disabled`, you cannot change the value at runtime. Changing the value of the `event_scheduler` variable requires the SUPER privilege.

Since [MariaDB 10.0.22](/kb/en/mariadb-10022-release-notes/), setting the [event_scheduler](/kb/en/server-system-variables/#event_scheduler) system variable will also try to reload the [mysql.event table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlevent-table/) if it was not properly loaded at startup.

## Viewing current events

A list of current events can be obtained with the [SHOW EVENTS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-events/) statement. This only shows the event name and interval - the full event details, including the SQL, can be seen by querying the [Information Schema EVENTS table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-events-table/), or with [SHOW CREATE EVENT](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-event/).

If an event is currently being executed, it can be seen by querying the [Information Schema PROCESSLIST table](/kb/en/information-schema-processlist-table/), or with the [SHOW PROCESSLIST](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-processlist/) statement.

### Example

```sql
SHOW EVENTS\G;
*************************** 1. row ***************************
                  Db: test
                Name: test_event
             Definer: root@localhost
           Time zone: SYSTEM
                Type: RECURRING
          Execute at: NULL
      Interval value: 1
      Interval field: MINUTE
              Starts: 2013-05-20 13:46:56
                Ends: NULL
              Status: ENABLED
          Originator: 1
character_set_client: utf8
collation_connection: utf8_general_ci
  Database Collation: latin1_swedish_ci
```

```sql
SHOW CREATE EVENT test_event\G
*************************** 1. row ***************************
               Event: test_event
            sql_mode: 
           time_zone: SYSTEM
        Create Event: CREATE DEFINER=`root`@`localhost` EVENT `test_event` ON SCHEDULE EVERY 1 MINUTE STARTS '2013-05-20 13:46:56' ON COMPLETION NOT PRESERVE ENABLE DO UPDATE test.t1 SET a = a + 1
character_set_client: utf8
collation_connection: utf8_general_ci
  Database Collation: latin1_swedish_ci
```

## Altering events

An event can be changed with the [ALTER EVENT](/programming-customizing-mariadb/triggers-events/event-scheduler/alter-event/) statement.

### Example

```sql
ALTER EVENT test_event ON SCHEDULE EVERY '2:3' DAY_HOUR;
```

## Dropping events

Events are dropped with the [DROP EVENT](/sql-statements-structure/sql-statements/data-definition/drop/drop-event/) statement. Events are also also automatically dropped once they have run for the final time according to their schedule, unless the ON COMPLETION PRESERVE clause has been specified.

### Example

```sql
DROP EVENT test_event;
Query OK, 0 rows affected (0.00 sec)
```

## See also

- [CREATE EVENT](/sql-statements-structure/sql-statements/data-definition/create/create-event/)
- [SHOW CREATE EVENT](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-event/)
- [ALTER EVENT](/programming-customizing-mariadb/triggers-events/event-scheduler/alter-event/)
- [DROP EVENT](/sql-statements-structure/sql-statements/data-definition/drop/drop-event/)