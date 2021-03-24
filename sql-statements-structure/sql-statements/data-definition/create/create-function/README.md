# CREATE FUNCTION

## Syntax

```sql
CREATE [OR REPLACE]
    [DEFINER = {user | CURRENT_USER | role | CURRENT_ROLE }]
    [AGGREGATE] FUNCTION [IF NOT EXISTS] func_name ([func_parameter[,...]])
    RETURNS type
    [characteristic ...]
    RETURN func_body
func_parameter:
    param_name type
type:
    Any valid MariaDB data type
characteristic:
    LANGUAGE SQL
  | [NOT] DETERMINISTIC
  | { CONTAINS SQL | NO SQL | READS SQL DATA | MODIFIES SQL DATA }
  | SQL SECURITY { DEFINER | INVOKER }
  | COMMENT 'string'
func_body:
    Valid SQL procedure statement
```

## Description

Use the `CREATE FUNCTION` statement to create a new [stored function](/programming-customizing-mariadb/stored-routines/stored-functions/). You must have
the [CREATE ROUTINE](/kb/en/grant/#database-privileges) database privilege to use `CREATE FUNCTION`.
A function takes any number of arguments and returns a value from the function body. The
function body can be any valid SQL expression as you would use, for example, in any select
expression. If you have the appropriate privileges, you can call the function exactly as you
would any built-in function. See [Security](#security) below for details on privileges.

You can also use a variant of the `CREATE FUNCTION` statement to install a user-defined
function (UDF) defined by a plugin. See [CREATE FUNCTION (UDF)](/programming-customizing-mariadb/user-defined-functions/create-function-udf/)
for details.

You can use a [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/) statement for the function body by enclosing it in
parentheses, exactly as you would to use a subselect for any other expression. The `SELECT`
statement must return a single value. If more than one column is returned when the function is called,
error 1241 results. If more than one row is returned when the function is called, error 1242
results. Use a `LIMIT` clause to ensure only one row is returned.

You can also replace the `RETURN` clause with a [BEGIN...END](/programming-customizing-mariadb/programmatic-compound-statements/begin-end/) compound
statement. The compound statement must contain a `RETURN` statement. When the function is
called, the `RETURN` statement immediately returns its result, and any statements after `RETURN`
are effectively ignored.

By default, a function is associated with the current database. To associate the function explicitly
with a given database, specify the fully-qualified name as `<em>db_name</em>.<em>func_name</em>`
when you create it. If the function name is the same as the name of a built-in function, you must
use the fully qualified name when you call it.

The parameter list enclosed within parentheses must always be present.
If there are no parameters, an empty parameter list of () should be
used. Parameter names are not case sensitive.

Each parameter can be declared to use any valid data type, except that
the COLLATE attribute cannot be used.

For valid identifiers to use as function names, see [Identifier Names](/sql-statements-structure/sql-language-structure/identifier-names/).

#### AGGREGATE

##### MariaDB starting with [10.3.3](/kb/en/mariadb-1033-release-notes/)

From [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/), it is possible to create stored aggregate functions as well. See [Stored Aggregate Functions](/programming-customizing-mariadb/stored-routines/stored-functions/stored-aggregate-functions/) for details.

#### RETURNS

The `RETURNS` clause specifies the return type of the function. `NULL` values are permitted with all return types.

What happens if the `RETURN` clause returns a value of a different type? It depends on the [SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode/) in effect at the moment of the function creation.

If the SQL_MODE is strict (STRICT_ALL_TABLES or STRICT_TRANS_TABLES flags are specified), a 1366 error will be produced.

Otherwise, the value is coerced to the proper type. For example, if a function
specifies an `ENUM` or `SET` value in the `RETURNS` clause, but the `RETURN`
clause returns an integer, the value returned from the function is the string for the corresponding `ENUM`
member of set of `SET` members.

MariaDB stores the SQL_MODE system variable setting that is in effect at the
time a routine is created, and always executes the routine with this setting in
force, regardless of the server SQL mode in effect when the routine is invoked.

#### LANGUAGE SQL

`LANGUAGE SQL` is a standard SQL clause, and it can be used in MariaDB for portability. However that clause has no meaning, because SQL is the only supported language for stored functions.

A function is deterministic if it can produce only one result for a given list of parameters. If the result may be affected by stored data, server variables, random numbers or any value that is not explicitly passed, then the function is not deterministic. Also, a function is non-deterministic if it uses non-deterministic functions like [NOW()](/built-in-functions/date-time-functions/now/) or [CURRENT_TIMESTAMP()](/built-in-functions/secondary-functions/information-functions/current_user/). The optimizer may choose a faster execution plan if it known that the function is deterministic. In such cases, you should declare the routine using the `DETERMINISTIC` keyword. If you want to explicitly state that the function is not deterministic (which is the default) you can use the `NOT DETERMINISTIC` keywords.

If you declare a non-deterministic function as `DETERMINISTIC`, you may get incorrect results. If you declare a deterministic function as `NOT DETERMINISTIC`, in some cases the queries will be slower.

#### OR REPLACE

##### MariaDB starting with [10.1.3](/kb/en/mariadb-1013-release-notes/)

If the optional `OR REPLACE` clause is used, it acts as a shortcut for:

```sql
DROP FUNCTION IF EXISTS function_name;
CREATE FUNCTION function_name ...;
```

with the exception that any existing [privileges](/programming-customizing-mariadb/stored-routines/stored-functions/stored-routine-privileges/) for the function are not dropped.

#### IF NOT EXISTS

##### MariaDB starting with [10.1.3](/kb/en/mariadb-1013-release-notes/)

If the IF NOT EXISTS clause is used, MariaDB will return a warning instead of an error if the function already exists. Cannot be used together with OR REPLACE.

#### [NOT] DETERMINISTIC

The `[NOT] DETERMINISTIC` clause also affects [binary logging](/mariadb-administration/server-monitoring-logs/binary-log/), because the `STATEMENT` format can not be used to store or replicate non-deterministic statements.

`CONTAINS SQL`, `NO SQL`, `READS SQL DATA`, and `MODIFIES SQL DATA` are informative clauses that tell the server what the function does. MariaDB does not check in any way whether the specified clause is correct. If none of these clauses are specified, `CONTAINS SQL` is used by default.

#### MODIFIES SQL DATA

`MODIFIES SQL DATA` means that the function contains statements that may modify data stored in databases. This happens if the function contains statements like [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete/), [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update/), [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert/), [REPLACE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/replace/) or DDL.

#### READS SQL DATA

`READS SQL DATA` means that the function reads data stored in databases, but does not modify any data. This happens if [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/) statements are used, but there no write operations are executed.

#### CONTAINS SQL

`CONTAINS SQL` means that the function contains at least one SQL statement, but it does not read or write any data stored in a database. Examples include [SET](/sql-statements-structure/sql-statements/administrative-sql-statements/set-commands/set/) or [DO](/sql-statements-structure/sql-statements/stored-routine-statements/do/).

#### NO SQL

`NO SQL` means nothing, because MariaDB does not currently support any language other than SQL.

#### Oracle Mode

##### MariaDB starting with [10.3](/kb/en/what-is-mariadb-103/)

From [MariaDB 10.3](/kb/en/what-is-mariadb-103/), a subset of Oracle's PL/SQL language has been supported in addition to the traditional SQL/PSM-based MariaDB syntax. See [Oracle mode from MariaDB 10.3](/kb/en/sql_modeoracle-from-mariadb-103/#stored-procedures-and-stored-functions) for details on changes when running Oracle mode.

## Security

You must have the [EXECUTE](/kb/en/grant/#function-privileges) privilege on a function to call it.
MariaDB automatically grants the `EXECUTE` and `ALTER ROUTINE` privileges to the
account that called `CREATE FUNCTION`, even if the `DEFINER` clause was used.

Each function has an account associated as the definer. By default, the definer is the account
that created the function. Use the `DEFINER` clause to specify a different account as the
definer. You must have the [SUPER](/kb/en/grant/#super) privilege, or, from [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), the [SET USER](/kb/en/grant/#set-user) privilege, to use the `DEFINER`
clause. See [Account Names](/kb/en/create-user/#account-names) for details on specifying accounts.

The `SQL SECURITY` clause specifies what privileges are used when a function is called.
If `SQL SECURITY` is `INVOKER`, the function body will be evaluated using the privileges
of the user calling the function. If `SQL SECURITY` is `DEFINER`, the function body is
always evaluated using the privileges of the definer account. `DEFINER` is the default.

This allows you to create functions that grant limited access to certain data. For example, say
you have a table that stores some employee information, and that you've granted `SELECT`
privileges [only on certain columns](/kb/en/grant/#column-privileges) to the user account `roger`.

```sql
CREATE TABLE employees (name TINYTEXT, dept TINYTEXT, salary INT);
GRANT SELECT (name, dept) ON employees TO roger;
```

To allow the user the get the maximum salary for a department, define a function and grant
the `EXECUTE` privilege:

```sql
CREATE FUNCTION max_salary (dept TINYTEXT) RETURNS INT RETURN
  (SELECT MAX(salary) FROM employees WHERE employees.dept = dept);
GRANT EXECUTE ON FUNCTION max_salary TO roger;
```

Since `SQL SECURITY` defaults to `DEFINER`, whenever the user `roger` calls
this function, the subselect will execute with your privileges. As long as you have privileges to
select the salary of each employee, the caller of the function will be able to get the maximum
salary for each department without being able to see individual salaries.

## Character sets and collations

Function return types can be declared to use any valid [character set and collation](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/). If used, the COLLATE attribute needs to be preceded by a CHARACTER SET attribute.

If the character set and collation are not specifically set in the statement, the database defaults at the time of creation will be used. If the database defaults change at a later stage, the stored function character set/collation will not be changed at the same time; the stored function needs to be dropped and recreated to ensure the same character set/collation as the database is used.

## Examples

The following example function takes a parameter, performs an operation using
an SQL function, and returns the result.

```sql
CREATE FUNCTION hello (s CHAR(20))
    RETURNS CHAR(50) DETERMINISTIC
    RETURN CONCAT('Hello, ',s,'!');

SELECT hello('world');
+----------------+
| hello('world') |
+----------------+
| Hello, world!  |
+----------------+
```

You can use a compound statement in a function to manipulate data with statements
like `INSERT` and `UPDATE`. The following example creates a counter function
that uses a temporary table to store the current value. Because the compound statement
contains statements terminated with semicolons, you have to first change the statement
delimiter with the `DELIMITER` statement to allow the semicolon to be used in the
function body. See [Delimiters in the mysql client](/kb/en/delimiters-in-the-mysql-client/) for more.

```sql
CREATE TEMPORARY TABLE counter (c INT);
INSERT INTO counter VALUES (0);
DELIMITER //
CREATE FUNCTION counter () RETURNS INT
  BEGIN
    UPDATE counter SET c = c + 1;
    RETURN (SELECT c FROM counter LIMIT 1);
  END //
DELIMITER ;
```

Character set and collation:

```sql
CREATE FUNCTION hello2 (s CHAR(20))
  RETURNS CHAR(50) CHARACTER SET 'utf8' COLLATE 'utf8_bin' DETERMINISTIC
  RETURN CONCAT('Hello, ',s,'!');
```

## See Also

- [Identifier Names](/sql-statements-structure/sql-language-structure/identifier-names/)
- [Stored Aggregate Functions](/programming-customizing-mariadb/stored-routines/stored-functions/stored-aggregate-functions/)
- [CREATE FUNCTION (UDF)](/programming-customizing-mariadb/user-defined-functions/create-function-udf/)
- [SHOW CREATE FUNCTION](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-function/)
- [ALTER FUNCTION](/sql-statements-structure/sql-statements/data-definition/alter/alter-function/)
- [DROP FUNCTION](/programming-customizing-mariadb/stored-routines/stored-functions/drop-function/)
- [SHOW FUNCTION STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-function-status/)
- [Stored Routine Privileges](/programming-customizing-mariadb/stored-routines/stored-functions/stored-routine-privileges/)
- [Information Schema ROUTINES Table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-routines-table/)