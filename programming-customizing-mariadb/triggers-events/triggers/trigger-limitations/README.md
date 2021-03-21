# Trigger Limitations

The following restrictions apply to [triggers](/programming-customizing-mariadb/triggers-events/triggers).

- All of the restrictions listed in [Stored Routine Limitations](/programming-customizing-mariadb/stored-routines/stored-routine-limitations).
- All of the restrictions listed in [Stored Function Limitations](/programming-customizing-mariadb/stored-routines/stored-functions/stored-function-limitations).
- Until [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/), each table can have only one trigger for each timing/event combination (ie: you can't define two BEFORE INSERT triggers for the same table).
- Triggers are always executed for each row. The standard `FOR EACH STATEMENT` option is not supported in MariaDB,
- Triggers cannot operate on any tables in the mysql, information_schema or performance_schema database.
- Cannot return a resultset.
- The [RETURN](/programming-customizing-mariadb/programmatic-compound-statements/return) statement is not permitted, since triggers don't return any values. Use [LEAVE](/programming-customizing-mariadb/programmatic-compound-statements/leave) to immediately exit a trigger.
- Triggers are not activated by [foreign key](/replication/optimization-and-tuning/optimization-and-indexes/foreign-keys) actions.
- If a trigger is loaded into cache, it is not automatically reloaded when the table metadata changes. In this case a trigger can operate using the outdated metadata.
- By default, with row-based replication, triggers run on the master, and the effects of their executions are replicated to the slaves. However, starting from [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/), it is possible to run triggers on the slaves. See [Running triggers on the slave for Row-based events](/replication/standard-replication/running-triggers-on-the-slave-for-row-based-events).

## See Also

- [Trigger Overview](/programming-customizing-mariadb/triggers-events/triggers/trigger-overview)
- [CREATE TRIGGER](/programming-customizing-mariadb/triggers-events/triggers/create-trigger)
- [DROP TRIGGER](/sql-statements-structure/sql-statements/data-definition/drop/drop-trigger)
- [Information Schema TRIGGERS Table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-triggers-table)
- [SHOW TRIGGERS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-triggers)
- [SHOW CREATE TRIGGER](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-trigger)