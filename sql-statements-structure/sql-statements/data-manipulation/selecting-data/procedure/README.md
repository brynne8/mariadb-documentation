# PROCEDURE

The `PROCEDURE` clause of [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select) passes the whole result set to a Procedure which will process it. These Procedures are not [Stored Procedures](/programming-customizing-mariadb/stored-routines/stored-procedures), and can only be written in the C language, so it is necessary to recompile the server.

Currently, the only available procedure is [ANALYSE](/built-in-functions/secondary-functions/information-functions/procedure-analyse), which examines the resultset and suggests the optimal datatypes for each column. It is defined in the `sql/sql_analyse.cc` file, and can be used as an example to create more Procedures.

This clause cannot be used in a [view](/programming-customizing-mariadb/views)'s definition.

## See Also

- [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select)
- [Stored Procedures](/programming-customizing-mariadb/stored-routines/stored-procedures)