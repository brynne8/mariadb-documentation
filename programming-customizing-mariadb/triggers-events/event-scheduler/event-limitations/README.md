# Event Limitations

The following restrictions apply to [Events](/kb/en/stored-programs-and-views-events/).

- All of the restrictions listed in [Stored Routine Limitations](/programming-customizing-mariadb/stored-routines/stored-routine-limitations).
- Events cannot return a resultset.
- Event names are case insensitive, so it's not possible to define two events in the same database if their case insensitive names will match. This restriction has applied since MariaDB/MySQL 5.1.8. If you are upgrading from an older version of MySQL, and have events that could clash, these events need to be renamed before the upgrade.
- Events do not support dates beyond the maximum that can be represented in the Unix epoch (2038-01-19).
- Events cannot be created, dropped or altered by another stored program, trigger or event.
- Events cannot create, drop or alter stored programs or triggers
- Event timings cannot be strictly predicted. The intervals MONTH, YEAR_MONTH, QUARTER and YEAR are all resolved in months. All others are resolved in seconds. A delay of up to two seconds is possible in extreme cases, and events scheduled to run at the same second cannot be executed in a given order. The `LAST_EXECUTED` column in the [INFORMATION_SCHEMA.EVENTS](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-events-table) table will however always be accurate to within a second.
- A new connection is used for each execution of statements within the body of an event, so the session counts for [server status variables](/replication/optimization-and-tuning/system-variables/server-status-variables) such as Com_delete and Com_select will not reflect these.