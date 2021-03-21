# DECLARE HANDLER

## Syntax

```sql
DECLARE handler_type HANDLER
    FOR condition_value [, condition_value] ...
    statement

handler_type:
    CONTINUE
  | EXIT 
  | UNDO

condition_value:
    SQLSTATE [VALUE] sqlstate_value
  | condition_name
  | SQLWARNING
  | NOT FOUND
  | SQLEXCEPTION
  | mariadb_error_code
```

## Description

The `DECLARE ... HANDLER` statement specifies handlers that each may
deal with one or more conditions. If one of these conditions occurs,
the specified statement is executed. statement can be a simple
statement (for example, `SET var_name = value`), or it can be a compound
statement written using [BEGIN and END](/programming-customizing-mariadb/programmatic-compound-statements/begin-end/).

Handlers must be declared after local variables, a `CONDITION` and a [CURSOR](/kb/en/programmatic-and-compound-statements-cursors/).

For a `CONTINUE` handler, execution of the current program continues
after execution of the handler statement. For an EXIT handler,
execution terminates for the [BEGIN ... END](/programming-customizing-mariadb/programmatic-compound-statements/begin-end/) compound statement in which
the handler is declared. (This is true even if the condition occurs in
an inner block.) The `UNDO` handler type statement is not supported.

If a condition occurs for which no handler has been declared, the
default action is `EXIT`.

A condition_value for `DECLARE ... HANDLER` can be any of the following
values:

- An [SQLSTATE](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-diagnostics/sqlstate/) value (a 5-character string literal) or a MariaDB error
code (a number). You should not use `SQLSTATE` value '00000' or MariaDB
error code 0, because those indicate sucess rather than an error
condition. For a list of `SQLSTATE` values and MariaDB error codes, see
[MariaDB Error Codes](/sql-statements-structure/sql-language-structure/mariadb-error-codes/).
- A condition name previously specified with `DECLARE ... CONDITION`. It must be in the same stored program. See [DECLARE CONDITION](/programming-customizing-mariadb/programmatic-compound-statements/declare-condition/).
- `SQLWARNING` is shorthand for the class of SQLSTATE values that begin
with '01'.
- `NOT FOUND` is shorthand for the class of `SQLSTATE` values that begin
with '02'. This is relevant only the context of cursors and is used to
control what happens when a cursor reaches the end of a data set. If
no more rows are available, a No Data condition occurs with `SQLSTATE`
value 02000. To detect this condition, you can set up a handler for it
(or for a `NOT FOUND` condition). An example is shown in [Cursor Overview](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-cursors/cursor-overview/). This condition also occurs for [SELECT ... INTO](/kb/en/select-into/) var_list statements that retrieve no
rows.
- SQLEXCEPTION is shorthand for the class of SQLSTATE values that do
not begin with '00', '01', or '02'.

When an error raises, in some cases it could be handled by multiple `HANDLER`s. For example, there may be an handler for 1050 error, a separate handler for the 42S01 SQLSTATE, and another separate handler for the `SQLEXCEPTION` class: in theory all occurrences of `HANDLER` may catch the 1050 error, but MariaDB chooses the `HANDLER` with the highest precedence. Here are the precedence rules:

- Handlers which refer to an error code have the highest precedence.
- Handlers which refer to a SQLSTATE come next.
- Handlers which refer to an error class have the lowest precedence.

In some cases, a statement could produce multiple errors. If this happens, in some cases multiple handlers could have the highest precedence. In such cases, the choice of the handler is indeterminate.

Note that if an error occurs within a `CONTINUE HANDLER` block, it can be handled by another `HANDLER`. However, a `HANDLER` which is already in the stack (that is, it has been called to handle an error and its execution didn't finish yet) cannot handle new errors<span>—</span>this prevents endless loops. For example, suppose that a stored procedure contains a `CONTINUE HANDLER` for `SQLWARNING` and another `CONTINUE HANDLER` for `NOT FOUND`. At some point, a NOT FOUND error occurs, and the execution enters the `NOT FOUND HANDLER`. But within that handler, a warning occurs, and the execution enters the `SQLWARNING HANDLER`. If another `NOT FOUND` error occurs, it cannot be handled again by the `NOT FOUND HANDLER`, because its execution is not finished.

When a `DECLARE HANDLER` block can handle more than one error condition, it may be useful to know which errors occurred. To do so, you can use the [GET DIAGNOSTICS](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-diagnostics/get-diagnostics/) statement.

An error that is handled by a `DECLARE HANDLER` construct can be issued again using the [RESIGNAL](/programming-customizing-mariadb/programmatic-compound-statements/resignal/) statement.

Below is an example using `DECLARE HANDLER`:

```sql
CREATE TABLE test.t (s1 INT, PRIMARY KEY (s1));

DELIMITER //

CREATE PROCEDURE handlerdemo ( )
     BEGIN
       DECLARE CONTINUE HANDLER FOR SQLSTATE '23000' SET @x2 = 1;
       SET @x = 1;
       INSERT INTO test.t VALUES (1);
       SET @x = 2;
       INSERT INTO test.t VALUES (1);
       SET @x = 3;
     END;
     //

DELIMITER ;

CALL handlerdemo( );

SELECT @x;
+------+
| @x   |
+------+
|    3 |
+------+
```