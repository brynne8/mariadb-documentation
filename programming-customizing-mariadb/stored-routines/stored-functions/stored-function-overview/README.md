# Stored Function Overview

A Stored Function is a defined function that is called from within an SQL statement like a regular function, and returns a single value.
<br>
<br>

## Creating Stored Functions

Here's a skeleton example to see a stored function in action:
<br>
<br>

```sql
DELIMITER //

CREATE FUNCTION FortyTwo() RETURNS TINYINT DETERMINISTIC
BEGIN
 DECLARE x TINYINT;
 SET x = 42;
 RETURN x;
END 

//

DELIMITER ;
```

First, the delimiter is changed, since the function definition will contain the regular semicolon delimiter. See [Delimiters in the mysql client](/kb/en/delimiters-in-the-mysql-client/) for more. Then the function is named `FortyTwo` and defined to return a `tinyin`. The `DETERMINISTIC` keyword is not necessary in all cases (although if binary logging is on, leaving it out will throw an error), and is to help the query optimizer choose a query plan. A deterministic function is one that, given the same arguments, will always return the same result.

Next, the function body is placed between [BEGIN and END](/programming-customizing-mariadb/programmatic-compound-statements/begin-end) statements. It declares a tinyint, `X`, which is simply set to 42, and this is the result returned.

```sql
SELECT FortyTwo();
+------------+
| FortyTwo() |
+------------+
|         42 |
+------------+
```

Of course, a function that doesn't take any arguments is of little use. Here's a more complex example:

```sql
DELIMITER //
CREATE FUNCTION VatCents(price DECIMAL(10,2)) RETURNS INT DETERMINISTIC
BEGIN
 DECLARE x INT;
 SET x = price * 114;
 RETURN x;
END //
Query OK, 0 rows affected (0.04 sec)
DELIMITER ;
```

This function takes an argument, `price` which is defined as a DECIMAL, and returns an INT.

Take a look at the [CREATE FUNCTION](/sql-statements-structure/sql-statements/data-definition/create/create-function) page for more details.

From [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/), it is also possible to create [stored aggregate functions](/programming-customizing-mariadb/stored-routines/stored-functions/stored-aggregate-functions).

## Stored Function listings and definitions

To find which stored functions are running on the server, use [SHOW FUNCTION STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-function-status).

```sql
SHOW FUNCTION STATUS\G
*************************** 1. row ***************************
                  Db: test
                Name: VatCents
                Type: FUNCTION
             Definer: root@localhost
            Modified: 2013-06-01 12:40:31
             Created: 2013-06-01 12:40:31
       Security_type: DEFINER
             Comment: 
character_set_client: utf8
collation_connection: utf8_general_ci
  Database Collation: latin1_swedish_ci
1 row in set (0.00 sec)
```

or query the [routines table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-routines-table) in the INFORMATION_SCHEMA database directly:

```sql
SELECT ROUTINE_NAME FROM INFORMATION_SCHEMA.ROUTINES WHERE
  ROUTINE_TYPE='FUNCTION';
+--------------+
| ROUTINE_NAME |
+--------------+
| VatCents     |
+--------------+
```

To find out what the stored function does, use [SHOW CREATE FUNCTION](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-function).

```sql
SHOW CREATE FUNCTION VatCents\G
*************************** 1. row ***************************
            Function: VatCents
            sql_mode: 
     Create Function: CREATE DEFINER=`root`@`localhost` FUNCTION `VatCents`(price DECIMAL(10,2)) RETURNS int(11)
    DETERMINISTIC
BEGIN
 DECLARE x INT;
 SET x = price * 114;
 RETURN x;
END
character_set_client: utf8
collation_connection: utf8_general_ci
  Database Collation: latin1_swedish_ci
```

## Dropping and Updating Stored Functions

To drop a stored function, use the [DROP FUNCTION](/programming-customizing-mariadb/stored-routines/stored-functions/drop-function) statement.

```sql
DROP FUNCTION FortyTwo;
```

To change the characteristics of a stored function, use [ALTER FUNCTION](/sql-statements-structure/sql-statements/data-definition/alter/alter-function). Note that you cannot change the parameters or body of a stored function using this statement; to make such changes, you must drop and re-create the function using DROP FUNCTION and CREATE FUNCTION.

## Permissions in Stored Functions

See the article [Stored Routine Privileges](/programming-customizing-mariadb/stored-routines/stored-functions/stored-routine-privileges).

## See Also

- [CREATE FUNCTION](/sql-statements-structure/sql-statements/data-definition/create/create-function)
- [SHOW CREATE FUNCTION](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-function)
- [DROP FUNCTION](/programming-customizing-mariadb/stored-routines/stored-functions/drop-function)
- [Stored Routine Privileges](/programming-customizing-mariadb/stored-routines/stored-functions/stored-routine-privileges)
- [SHOW FUNCTION STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-function-status)
- [Information Schema ROUTINES Table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-routines-table)
- [Stored Aggregate Functions](/programming-customizing-mariadb/stored-routines/stored-functions/stored-aggregate-functions).