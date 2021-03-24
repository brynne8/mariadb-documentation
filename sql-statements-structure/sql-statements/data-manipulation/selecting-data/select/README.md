# SELECT

## Syntax

```sql
SELECT
    [ALL | DISTINCT | DISTINCTROW]
    [HIGH_PRIORITY]
    [STRAIGHT_JOIN]
    [SQL_SMALL_RESULT] [SQL_BIG_RESULT] [SQL_BUFFER_RESULT]
    [SQL_CACHE | SQL_NO_CACHE] [SQL_CALC_FOUND_ROWS]
    select_expr [, select_expr ...]
    [ FROM table_references
      [WHERE where_condition]
      [GROUP BY {col_name | expr | position} [ASC | DESC], ... [WITH ROLLUP]]
      [HAVING where_condition]
      [ORDER BY {col_name | expr | position} [ASC | DESC], ...]
      [LIMIT {[offset,] row_count | row_count OFFSET offset}]
      procedure|[PROCEDURE procedure_name(argument_list)]
      [INTO OUTFILE 'file_name' [CHARACTER SET charset_name] [export_options]
INTO DUMPFILE 'file_name'INTO var_name [, var_name] ]

      [[FOR UPDATE | LOCK IN SHARE MODE] [WAIT n | NOWAIT] ] ]
export_options:
    [{FIELDS | COLUMNS}
        [TERMINATED BY 'string']
        [[OPTIONALLY] ENCLOSED BY 'char']
        [ESCAPED BY 'char']
    ]
    [LINES
        [STARTING BY 'string']
        [TERMINATED BY 'string']
    ]
```

## Description

`SELECT` is used to retrieve rows selected from one or more
tables, and can include [UNION](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/joins-subqueries/union/) statements and [subqueries](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/joins-subqueries/subqueries/).

- Each <em>select_expr</em> expression indicates a column or data that you want to retrieve. You
must have at least one select expression. See [Select Expressions](#select-expressions) below.

- The `FROM` clause indicates the table or tables from which to retrieve rows.
Use either a single table name or a `JOIN` expression. See <a undefined>JOIN</a>
for details. If no table is involved, [FROM DUAL](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/dual/) can be specified.

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

The PARTITION clause was introduced in [MariaDB 10.0](/kb/en/what-is-mariadb-100/). See [Partition Pruning and Selection](/mariadb-administration/partitioning-tables/partition-pruning-and-selection/) for details.

- Each table can also be specified as `db_name`.`tabl_name`. Each column can also be specified as `tbl_name`.`col_name` or even `db_name`.`tbl_name`.`col_name`. This allows to write queries which involve multiple databases. See [Identifier Qualifiers](/sql-statements-structure/sql-language-structure/identifier-qualifiers/) for syntax details.

- The <code class="fixed" style="white-space:pre-wrap">WHERE</code> clause, if given, indicates the condition or
  conditions that rows must satisfy to be selected.
  <code class="fixed" style="white-space:pre-wrap">where_condition</code> is an expression that evaluates to true for
  each row to be selected. The statement selects all rows if there is no WHERE
  clause.
<ul start="1"><li>In the <code class="fixed" style="white-space:pre-wrap">WHERE</code> clause, you can use any of the functions and
   operators that MariaDB supports, except for aggregate (summary) functions. See [Functions and Operators](/kb/en/functions-and-operators/) and [Functions and Modifiers for use with GROUP BY](/kb/en/functions-and-modifiers-for-use-with-group-by/) (aggregate).
</li></ul>

- Use the [ORDER BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/order-by/) clause to order the results.

- Use the [LIMIT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/limit/) clause allows you to restrict the results to only
a certain number of rows, optionally with an offset.

- Use the [GROUP BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/group-by/) and `HAVING` clauses to group
rows together when they have columns or computed values in common.

SELECT can also be used to retrieve rows computed without reference to
any table.

## Select Expressions

A `SELECT` statement must contain one or more select expressions, separated
by commas. Each select expression can be one of the following:

- The name of a column.
- Any expression using [functions and operators](/kb/en/functions-and-operators/).
- `*` to select all columns from all tables in the `FROM` clause.
- `tbl_name.*` to select all columns from just the table <em>tbl_name</em>.

When specifying a column, you can either use just the column name or qualify the column
name with  the name of the table using `tbl_name.col_name`. The qualified form is
useful if you are joining multiple tables in the `FROM` clause. If you do not qualify the
column names when selecting from multiple tables, MariaDB will try to find the column in
each table. It is an error if that column name exists in multiple tables.

You can quote column names using backticks. If you are qualifying column names
with table names, quote each part separately as ``tbl_name`.`col_name``.

If you use any [grouping functions](/kb/en/functions-and-modifiers-for-use-with-group-by/)
in any of the select expressions, all rows in your results will be implicitly grouped, as if
you had used `GROUP BY NULL`.

## `DISTINCT`

A query may produce some identical rows. By default, all rows are retrieved, even when their values are the same. To explicitly specify that you want to retrieve identical rows, use the `ALL` option. If you want duplicates to be removed from the resultset, use the `DISTINCT` option. `DISTINCTROW` is a synonym for `DISTINCT`. See also [COUNT DISTINCT](/built-in-functions/aggregate-functions/count-distinct/) and [SELECT UNIQUE in Oracle mode](/kb/en/sql_modeoracle-from-mariadb-103/#simple-syntax-compatibility).

## INTO

The `INTO` clause is used to specify that the query results should be written to a file or variable.

- [SELECT INTO OUTFILE](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select-into-outfile/) - formatting and writing the result to an external file.
- [SELECT INTO DUMPFILE](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select-into-dumpfile/) - binary-safe writing of the unformatted results to an external file.
- [SELECT INTO Variable](/kb/en/select-into-variable/) - selecting and setting variables.

The reverse of `SELECT INTO OUTFILE` is [LOAD DATA](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/load-data-into-tables-or-index/).

## WAIT/NOWAIT

Set the lock wait timeout. See [WAIT and NOWAIT](/sql-statements-structure/sql-statements/transactions/wait-and-nowait/).

## PROCEDURE

Passes the whole result set to a C Procedure. See [PROCEDURE](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/procedure/) and [PROCEDURE ANALYSE](/built-in-functions/secondary-functions/information-functions/procedure-analyse/) (the only built-in procedure not requiring the server to be recompiled).

## SQL_CALC_FOUND_ROWS

When `SQL_CALC_FOUND_ROWS` is used, then MariaDB will calculate how many rows would
have been in the result, if there would be no [LIMIT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/limit/) clause. The result can be found by calling the function [FOUND_ROWS()](/built-in-functions/secondary-functions/information-functions/found_rows/) in your next sql statement.

<br>

##### MariaDB starting with [10.1.2](/kb/en/mariadb-1012-release-notes/)

## `max_statement_time` clause

By using <a undefined>max_statement_time</a> in conjunction with [SET STATEMENT](/sql-statements-structure/sql-statements/administrative-sql-statements/set-commands/set-statement/), it is possible to limit the execution time of individual queries. For example:

```sql
SET STATEMENT max_statement_time=100 FOR 
  SELECT field1 FROM table_name ORDER BY field1;
```

## See Also

- [Getting Data from MariaDB](/kb/en/getting-data-from-mariadb/) (Beginner tutorial)
- [Joins and Subqueries](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/joins-subqueries/)
- [LIMIT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/limit/)
- [ORDER BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/order-by/)
- [GROUP BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/group-by/)
- [Common Table Expressions](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/common-table-expressions/)
- [SELECT WITH ROLLUP](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select-with-rollup/)
- [SELECT INTO OUTFILE](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select-into-outfile/)
- [SELECT INTO DUMPFILE](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select-into-dumpfile/)
- [FOR UPDATE](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/for-update/)
- [LOCK IN SHARE MODE](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/lock-in-share-mode/)
- [Optimizer Hints](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/optimizer-hints/)
- [Oracle mode from MariaDB 10.3](/kb/en/sql_modeoracle-from-mariadb-103/#simple-syntax-compatibility)