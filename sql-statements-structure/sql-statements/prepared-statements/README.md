# Prepared Statements

In addition to using prepared statements from the libmysqld, you can also do prepared statements from any client by using the text based prepared statement interface.

You first prepare the statement with [PREPARE](/sql-statements-structure/sql-statements/prepared-statements/prepare-statement/), execute with [EXECUTE](/sql-statements-structure/sql-statements/prepared-statements/execute-statement/), and release it with [DEALLOCATE](/kb/en/deallocate-drop-prepared-statement/).

- [PREPARE Statement](/sql-statements-structure/sql-statements/prepared-statements/prepare-statement/) — Define a prepare statement.
- [Out Parameters in PREPARE](/sql-statements-structure/sql-statements/prepared-statements/out-parameters-in-prepare/) — Using question mark placeholders for out-parameters in the PREPARE statement
- [EXECUTE Statement](/sql-statements-structure/sql-statements/prepared-statements/execute-statement/) — Executes a previously PREPAREd statement
- [DEALLOCATE / DROP PREPARE](/sql-statements-structure/sql-statements/prepared-statements/deallocate-drop-prepare/) — Deallocates a prepared statement.
- [EXECUTE IMMEDIATE](/sql-statements-structure/sql-statements/prepared-statements/execute-immediate/) — Immediately execute a dynamic SQL statement