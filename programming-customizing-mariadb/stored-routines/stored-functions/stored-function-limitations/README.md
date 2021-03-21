# Stored Function Limitations

The following restrictions apply to [stored functions](/programming-customizing-mariadb/stored-routines/stored-functions).

- All of the restrictions listed in [Stored Routine Limitations](/programming-customizing-mariadb/stored-routines/stored-routine-limitations).
- Any statements that return a result set are not permitted. For example, a regular [SELECTs](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select) is not permitted, but a [SELECT INTO](/kb/en/select-into/) is. A cursor and [FETCH](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-cursors/fetch) statement is permitted.
- [FLUSH](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush) statements are not permitted.
- Statements that perform explicit or implicit commits or rollbacks are not permitted
- Cannot be used recursively.
- Cannot make changes to a table that is already in use (reading or writing) by the statement invoking the stored function.
- Cannot refer to a temporary table multiple times under different aliases, even in different statements.
- ROLLBACK TO SAVEPOINT and RELEASE SAVEPOINT statement which are in a stored function cannot refer to a savepoint which has been defined out of the current function.
- Prepared statements ([PREPARE](/sql-statements-structure/sql-statements/prepared-statements/prepare-statement), [EXECUTE](/sql-statements-structure/sql-statements/prepared-statements/execute-statement), [DEALLOCATE PREPARE](/kb/en/deallocate-drop-prepared-statement/)) cannot be used, and therefore nor can statements be constructed as strings and then executed.