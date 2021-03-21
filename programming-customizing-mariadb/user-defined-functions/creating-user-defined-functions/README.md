# Creating User-Defined Functions

[User-defined functions](/programming-customizing-mariadb/user-defined-functions/) allow MariaDB to be extended with a new function that works like a native (built-in) MariaDB function such as [ABS()](/built-in-functions/numeric-functions/abs/) or [CONCAT()](/built-in-functions/string-functions/concat/). There are alternative ways to add a new function: writing a native function (which requires modifying and compiling the server source code), or writing a stored function.

Statements making use of user-defined functions are not safe for replication.

Functions are written in C or C++, and to make use of them, the operating system must support dynamic loading.

Each new SQL function requires corresponding functions written in C/C++. In the list below, at least the main function - x() - and one other, are required. <em>x</em> should be replaced by the name of the function you are creating.

All functions need to be thread-safe, so not global or static variables that change can be allocated. Memory is allocated in <em>x_init()/ and freed in </em>x_deinit()<em>. </em>

## Simple Functions

### x()

Required for all UDFs; this is where the results are calculated.

<span class="cstm-style darkheader-nospace-borders"></span>

<table><tbody><tr><th>C/C++ type</th><th>SQL type</th></tr>
<tr><td>char *</td><td><a href="/kb/en/string-data-types/">STRING</a></td></tr>
<tr><td>long long</td><td><a href="/kb/en/integer/">INTEGER</a></td></tr>
<tr><td>double</td><td><a href="/kb/en/data-types-numeric-data-types/">REAL</a></td></tr>
</tbody></table>

<span class="cstm-style"></span>

DECIMAL functions return string values, and so should be written accordingly. It is not possible to create ROW functions.

### x_init()

Initialization function for x(). Can be used for the following:

- Check the number of arguments to X() (the SQL equivalent).
- Verify the argument types, or to force arguments to be of a particular type after the function is called.
- Specify whether the result can be NULL.
- Specify the maximum result length.
- For REAL functions, specify the maximum number of decimals for the result.
- Allocate any required memory.

### x_deinit()

De-initialization function for x(). Used to de-allocate memory that was allocated in x_init().

### Description

Each time the SQL function <em>X()</em> is called:

- MariaDB will first call the C/C++ initialization function, <em>x_init()</em>, assuming it exists. All setup will be performed, and if it returns an error, the SQL statement is aborted and no further functions are called.
- If there is no <em>x_init()</em> function, or it has been called and did not return an error, <em>x()</em> is then called once per row.
- After all rows have finished processing, <em>x_deinit()</em> is called, if present, to clean up by de-allocating any memory that was allocated in <em>x_init()</em>.
- See [User-defined Functions Calling Sequences](/programming-customizing-mariadb/user-defined-functions/user-defined-functions-calling-sequences/) for more details on the functions.

## Aggregate Functions

The following functions are required for aggregate functions, such as [AVG()](/built-in-functions/aggregate-functions/avg/) and [SUM()](/built-in-functions/aggregate-functions/sum/).

### x_clear()

Used to reset the current aggregate, but without inserting the argument as the initial aggregate value for the new group.

### x_add()

Used to add the argument to the current aggregate.

### x_remove()

Starting from [MariaDB 10.4](/kb/en/what-is-mariadb-104/), improves the support of [window functions](/built-in-functions/special-functions/window-functions/) (so it is not obligatory to add it) and should remove the argument from the current aggregate.

### Description

Each time the aggregate SQL function <em>X()</em> is called:

- MariaDB will first call the C/C++ initialization function, <em>x_init()</em>, assuming it exists. All setup will be performed, and if it returns an error, the SQL statement is aborted and no further functions are called.
- If there is no <em>x_init()</em> function, or it has been called and did not return an error, <em>x()</em> is then called once per row.
- After all rows have finished processing, <em>x_deinit()</em> is called, if present, to clean up by de-allocating any memory that was allocated in <em>x_init()</em>.

- MariaDB will first call the C/C++ initialization function, <em>x_init()</em>, assuming it exists. All setup will be performed, and if it returns an error, the SQL statement is aborted and no further functions are called.
- The table is sorted according to the [GROUP BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/group-by/) expression.
- <em>x_clear()</em> is called for the first row of each new group.
- <em>x_add()</em> is called once per row for each row in the same group.
- <em>x()</em> is called when the group changes, or after the last row, to get the aggregate result.
- The latter three steps are repeated until all rows have been processed.
- After all rows have finished processing, <em>x_deinit()</em> is called, if present, to clean up by de-allocating any memory that was allocated in <em>x_init()</em>.

## Examples

For an example, see `sql/udf_example.cc` in the source tree. For a collection of existing UDFs see [https://github.com/mysqludf](https://github.com/mysqludf).

## See Also

- [Stored Functions](/programming-customizing-mariadb/stored-routines/stored-functions/)
- [Stored Aggregate Functions](/programming-customizing-mariadb/stored-routines/stored-functions/stored-aggregate-functions/)
- [User-defined Functions Calling Sequences](/programming-customizing-mariadb/user-defined-functions/user-defined-functions-calling-sequences/)