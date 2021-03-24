# Stored Routine Privileges

It's important to give careful thought to the privileges associated with [stored functions](/programming-customizing-mariadb/stored-routines/stored-functions/) and [stored procedures](/programming-customizing-mariadb/stored-routines/stored-procedures/). The following is an explanation of how they work.

## Creating Stored Routines

- To create a stored routine, the <a undefined>CREATE ROUTINE</a> privilege is needed. The <a undefined>SUPER</a> privilege is required if a `DEFINER` is declared that's not the creator's account (see [DEFINER clause](#definer-clause) below). The `SUPER` privilege is also required if statement-based binary logging is used. See [Binary Logging of Stored Routines](/programming-customizing-mariadb/stored-routines/binary-logging-of-stored-routines/) for more details.

## Altering Stored Routines

- To make changes to, or drop, a stored routine, the <a undefined>ALTER ROUTINE</a> privilege is needed. The creator of a routine is temporarily granted this privilege if they attempt to change or drop a routine they created, unless the [automatic_sp_privileges](/kb/en/server-system-variables/#automatic_sp_privileges) variable is set to `0` (it defaults to 1).
- The `SUPER` privilege is also required if statement-based binary logging is used. See [Binary Logging of Stored Routines](/programming-customizing-mariadb/stored-routines/binary-logging-of-stored-routines/) for more details.

## Running Stored Routines

- To run a stored routine, the <a undefined>EXECUTE</a> privilege is needed. This is also temporarily granted to the creator if they attempt to run their routine unless the [automatic_sp_privileges](/kb/en/server-system-variables/#automatic_sp_privileges) variable is set to `0`.
- The <a undefined>SQL SECURITY clause</a> (by default `DEFINER`) specifies what privileges are used when a routine is called. If `SQL SECURITY` is `INVOKER`, the function body will be evaluated using the privileges of the user calling the function. If `SQL SECURITY` is `DEFINER`, the function body is always evaluated using the privileges of the definer account. `DEFINER` is the default. Thus, by default, users who can access the database associated with the stored routine can also run the routine, and potentially perform operations they wouldn't normally have permissions for.
- The creator of a routine is the account that ran the [CREATE FUNCTION](/sql-statements-structure/sql-statements/data-definition/create/create-function/) or [CREATE PROCEDURE](/programming-customizing-mariadb/stored-routines/stored-procedures/create-procedure/) statement, regardless of whether a `DEFINER` is provided. The definer is by default the creator unless otherwise specified.
- The server automatically changes the privileges in the [mysql.proc](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlproc-table/) table as required, but will not look out for manual changes.

### DEFINER Clause

If left out, the `DEFINER` is treated as the account that created the stored routine or view. If the account creating the routine has the `SUPER` privilege, another account can be specified as the `DEFINER`.

### SQL SECURITY Clause

This clause specifies the context the stored routine or view will run as. It can take two values - `DEFINER` or `INVOKER`. `DEFINER` is the account specified as the `DEFINER` when the stored routine or view was created (see the section above). `INVOKER` is the account invoking the routine or view.

As an example, let's assume a routine, created by a superuser who's specified as the `DEFINER`, deletes all records from a table. If `SQL SECURITY=DEFINER`, anyone running the routine, regardless of whether they have delete privileges, will be able to delete the records. If `SQL SECURITY = INVOKER`, the routine will only delete the records if the account invoking the routine has permission to do so.

`INVOKER` is usually less risky, as a user cannot perform any operations they're normally unable to. However, it's not uncommon for accounts to have relatively limited permissions, but be specifically granted access to routines, which are then invoked in the `DEFINER` context.

## Dropping Stored Routines

All privileges that are specific to a stored routine will be dropped when a [DROP FUNCTION](/programming-customizing-mariadb/stored-routines/stored-functions/drop-function/) or [DROP ROUTINE](drop-routine) is run. However, if a [CREATE OR REPLACE FUNCTION](/sql-statements-structure/sql-statements/data-definition/create/create-function/) or [CREATE OR REPLACE PROCEDURE](/programming-customizing-mariadb/stored-routines/stored-procedures/create-procedure/) is used to drop and replace and the routine, any privileges specific to that routine will not be dropped.

## See Also

- [Changing the DEFINER of MySQL stored routines etc.](https://mariadb.com/blog/changing-definer-mysql-stored-routines-etc) - maria.com post on what to do after you've dropped a user, and now want to change the DEFINER on all database objects that currently have it set to this dropped user.