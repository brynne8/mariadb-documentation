# User-Defined Functions

A user-defined function (UDF) is a way to extend MariaDB with a new function that works like a native (built-in) MariaDB function such as [ABS( )](/built-in-functions/numeric-functions/abs) or [CONCAT( )](/built-in-functions/string-functions/concat).

Statements making use of user-defined functions are not [safe for replication](/kb/en/unsafe-statements-for-replication/).

For an example, see `sql/udf_example.cc` in the source tree. For a collection of existing UDFs go to the [UDF Repository on GitHub](https://github.com/mysqludf/repositories).

There are alternative ways to add a new function: writing a native function, which requires modifying and compiling the server source code; or writing a [stored function](/programming-customizing-mariadb/stored-routines/stored-functions).

- [Creating User-Defined Functions](/programming-customizing-mariadb/user-defined-functions/creating-user-defined-functions/) — How to create user-defined functions in C/C++.
- [User-Defined Functions Calling Sequences](/programming-customizing-mariadb/user-defined-functions/user-defined-functions-calling-sequences/) — Declaring the functions required in a user-defined function.
- [User-Defined Functions Security](/programming-customizing-mariadb/user-defined-functions/user-defined-functions-security/) — MariaDB imposes a number of limitations on user-defined functions for security purposes.
- [CREATE FUNCTION UDF](/programming-customizing-mariadb/user-defined-functions/create-function-udf/) — Create a user-defined function.
- [DROP FUNCTION UDF](/programming-customizing-mariadb/user-defined-functions/drop-function-udf/) — Drop a user-defined function.
- [mysql.func Table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlfunc-table/) — User-defined function information