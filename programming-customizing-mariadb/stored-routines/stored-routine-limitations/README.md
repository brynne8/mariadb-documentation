# Stored Routine Limitations

The following SQL statements are not permitted inside any [stored routines](/kb/en/stored-programs-and-views/) ([stored functions](/programming-customizing-mariadb/stored-routines/stored-functions), [stored procedures](/programming-customizing-mariadb/stored-routines/stored-procedures), [events](/programming-customizing-mariadb/triggers-events/event-scheduler/events) or [triggers](/programming-customizing-mariadb/triggers-events/triggers)).

- [ALTER VIEW](/programming-customizing-mariadb/views/alter-view); you can use [CREATE OR REPLACE VIEW](/programming-customizing-mariadb/views/create-view) instead.
- [LOAD DATA](/kb/en/load-data/) and [LOAD TABLE](/kb/en/load-table-from-master/).
- [INSERT DELAYED](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-delayed) is permitted, but the statement is handled as a regular [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert).
- [LOCK TABLES](/sql-statements-structure/sql-statements/transactions/lock-tables) and [UNLOCK TABLES](/kb/en/unlock-tables/).
- References to [local variables](/programming-customizing-mariadb/programmatic-compound-statements/declare-variable) within prepared statements inside a stored routine (use [user-defined variables](/sql-statements-structure/sql-language-structure/user-defined-variables) instead).
- [BEGIN (WORK)](/sql-statements-structure/sql-statements/transactions/start-transaction) is treated as the beginning of a [BEGIN END](/programming-customizing-mariadb/programmatic-compound-statements/begin-end) block, not a transaction, so [START TRANSACTION](/sql-statements-structure/sql-statements/transactions/start-transaction) needs to be used instead.
- The number of permitted recursive calls is limited to [max_sp_recursion_depth](/kb/en/server-system-variables/#max_sp_recursion_depth). If this variable is 0 (default), recursivity is disabled. The limit does not apply to stored functions.
- Most statements that are not permitted in prepared statements are not permitted in stored programs. See [Prepare Statement:Permitted statements](/kb/en/prepare-statement/#permitted-statements) for a list of statements that can be used. [SIGNAL](/programming-customizing-mariadb/programmatic-compound-statements/signal), [RESIGNAL](/programming-customizing-mariadb/programmatic-compound-statements/resignal) and [GET DIAGNOSTICS](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-diagnostics/get-diagnostics) are exceptions, and may be used in stored routines.

There are also further limitations specific to the kind of stored routine.

Note that, if a stored program calls another stored program, the latter will inherit the caller's limitations. So, for example, if a stored procedure is called by a stored function, that stored procedure will not be able to produce a result set, because stored functions can't do this.

## See Also

- [Stored Function Limitations](/programming-customizing-mariadb/stored-routines/stored-functions/stored-function-limitations)
- [Trigger Limitations](/programming-customizing-mariadb/triggers-events/triggers/trigger-limitations)
- [Event Limitations](/programming-customizing-mariadb/triggers-events/event-scheduler/event-limitations)