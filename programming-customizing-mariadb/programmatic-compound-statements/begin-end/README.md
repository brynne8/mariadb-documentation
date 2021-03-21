# BEGIN END

## Syntax

```sql
[begin_label:] BEGIN [NOT ATOMIC]
    [statement_list]
END [end_label]
```

`NOT ATOMIC` is required when used outside of a [stored procedure](/programming-customizing-mariadb/stored-routines/stored-procedures/). Inside stored procedures or within an anonymous block, `BEGIN` alone starts a new anonymous block.

## Description

`BEGIN ... END` syntax is used for writing compound statements. A compound statement can contain multiple statements, enclosed by the `BEGIN` and `END` keywords. statement_list represents a list of one or more statements, each
terminated by a semicolon (i.e., `;`) statement delimiter. statement_list is
optional, which means that the empty compound statement (`BEGIN END`) is
legal.

Note that `END` will perform a commit. If you are running in [autocommit](/kb/en/server-system-variables/#autocommit) mode, every statement will be committed separately. If you are not running in `autocommit` mode, you must execute a [COMMIT](/sql-statements-structure/sql-statements/transactions/commit/) or [ROLLBACK](/sql-statements-structure/sql-statements/transactions/rollback/) after `END` to get the database up to date.

Use of multiple statements requires that a client is able to send statement strings containing the ; statement delimiter. This is handled in the [mysql](mysql-command-line_client)  command-line client with the [DELIMITER](/clients-utilities/mysql-client/delimiters/) command.
Changing the `;` end-of-statement delimiter (for example, to
<code class="highlight fixed" style="white-space:pre-wrap">//</code>) allows `;` to be used in a program body.

A compound statement within a [stored program](/kb/en/stored-programs-and-views/) can be
[labeled](/programming-customizing-mariadb/programmatic-compound-statements/labels/). `end_label` cannot be given unless `begin_label` also is present. If both are present, they must be the same.

`BEGIN ... END` constructs can be nested. Each block can define its own variables, a `CONDITION`, a `HANDLER` and a [CURSOR](/kb/en/programmatic-and-compound-statements-cursors/), which don't exist in the outer blocks. The most local declarations override the outer objects which use the same name (see example below).

The declarations order is the following:

- [`DECLARE` local variables](/programming-customizing-mariadb/programmatic-compound-statements/declare-variable/);
- [`DECLARE CONDITION`s](/programming-customizing-mariadb/programmatic-compound-statements/declare-condition/);
- [`DECLARE CURSOR`s](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-cursors/declare-cursor/);
- [`DECLARE HANDLER`s](/programming-customizing-mariadb/programmatic-compound-statements/declare-handler/);

Note that `DECLARE HANDLER` contains another `BEGIN ... END` construct.

Here is an example of a very simple, anonymous block:

```sql
BEGIN NOT ATOMIC
SET @a=1;
CREATE TABLE test.t1(a INT);
END|
```

Below is an example of nested blocks in a stored procedure:

```sql
CREATE PROCEDURE t( )
BEGIN
   DECLARE x TINYINT UNSIGNED DEFAULT 1;
   BEGIN
      DECLARE x CHAR(2) DEFAULT '02';
       DECLARE y TINYINT UNSIGNED DEFAULT 10;
       SELECT x, y;
   END;
   SELECT x;
END;
```

In this example, a [TINYINT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/tinyint/) variable, `x` is declared in the outter block. But in the inner block `x` is re-declared as a [CHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/char/) and an `y` variable is declared. The inner [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/)  shows the "new" value of `x`, and the value of `y`. But when x is selected in the outer block, the "old" value is returned. The final [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/) doesn't try to read `y`, because it doesn't exist in that context.

## See Also

- [Using compound statements outside of stored programs](/programming-customizing-mariadb/programmatic-compound-statements/using-compound-statements-outside-of-stored-programs/)
- [Changes in Oracle mode from MariaDB 10.3](/kb/en/sql_modeoracle-from-mariadb-103/#simple-syntax-compatibility)