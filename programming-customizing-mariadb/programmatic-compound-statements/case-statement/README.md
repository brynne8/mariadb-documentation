# CASE Statement

## Syntax

```sql
CASE case_value
    WHEN when_value THEN statement_list
    [WHEN when_value THEN statement_list] ...
    [ELSE statement_list]
END CASE
```

Or:

```sql
CASE
    WHEN search_condition THEN statement_list
    [WHEN search_condition THEN statement_list] ...
    [ELSE statement_list] 
END CASE
```

## Description

The text on this page describes the `CASE` statement for [stored programs](/kb/en/stored-programs-and-views/). See the [CASE OPERATOR](/built-in-functions/control-flow-functions/case-operator/) for details on the CASE operator outside of [stored programs](/kb/en/stored-programs-and-views/).

The `CASE` statement for [stored programs](/kb/en/stored-programs-and-views/) implements a complex conditional
construct. If a `search_condition` evaluates to true, the corresponding SQL
statement list is executed. If no search condition matches, the statement list
in the `ELSE` clause is executed. Each `statement_list` consists of one or
more statements.

The `CASE` statement cannot have an `ELSE NULL` clause, and it is
terminated with `END CASE` instead of `END`. implements a complex conditional
construct. If a `search_condition` evaluates to true, the corresponding SQL
statement list is executed. If no search condition matches, the statement list
in the `ELSE` clause is executed. Each `statement_list` consists of one or
more statements.

If no when_value or search_condition matches the value tested and the `CASE`
statement contains no `ELSE` clause, a Case not found for `CASE` statement
error results.

Each statement_list consists of one or more statements; an empty
`statement_list` is not allowed. To handle situations where no value is
matched by any `WHEN` clause, use an `ELSE` containing an
empty [BEGIN ... END](/programming-customizing-mariadb/programmatic-compound-statements/begin-end/) block, as shown in this example:

```sql
DELIMITER |
CREATE PROCEDURE p()
BEGIN
  DECLARE v INT DEFAULT 1;
  CASE v
    WHEN 2 THEN SELECT v;
    WHEN 3 THEN SELECT 0;
    ELSE BEGIN END;
  END CASE;
END;
|
```

The indentation used here in the `ELSE` clause is for purposes of clarity only,
and is not otherwise significant. See [Delimiters in the mysql client](/kb/en/delimiters-in-the-mysql-client/) for more on the use of the delimiter command.

<strong>Note:</strong> The syntax of the `CASE` statement used inside stored programs
differs slightly from that of the SQL CASE expression described in
[CASE OPERATOR](/built-in-functions/control-flow-functions/case-operator/).
The `CASE` statement cannot have an `ELSE NULL` clause, and it is
terminated with `END CASE` instead of `END`.