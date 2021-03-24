# JOIN Syntax

## Description

MariaDB supports the following <code class="fixed" style="white-space:pre-wrap">JOIN</code> syntaxes for
the `table_references` part of [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/) statements and
multiple-table [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete/) and [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update/) statements:

```sql
table_references:
    table_reference [, table_reference] ...

table_reference:
    table_factor
  | join_table

table_factor:
    tbl_name [PARTITION (partition_list)]
        [query_system_time_period_specification] [[AS] alias] [index_hint_list]
  | table_subquery [query_system_time_period_specification] [AS] alias
  | ( table_references )
  | { ON table_reference LEFT OUTER JOIN table_reference
        ON conditional_expr }

join_table:
    table_reference [INNER | CROSS] JOIN table_factor [join_condition]
  | table_reference STRAIGHT_JOIN table_factor
  | table_reference STRAIGHT_JOIN table_factor ON conditional_expr
  | table_reference {LEFT|RIGHT} [OUTER] JOIN table_reference join_condition
  | table_reference NATURAL [{LEFT|RIGHT} [OUTER]] JOIN table_factor

join_condition:
    ON conditional_expr
  | USING (column_list)

query_system_time_period_specification:
    FOR SYSTEM_TIME AS OF point_in_time
  | FOR SYSTEM_TIME BETWEEN point_in_time AND point_in_time
  | FOR SYSTEM_TIME FROM point_in_time TO point_in_time
  | FOR SYSTEM_TIME ALL

point_in_time:
    [TIMESTAMP] expression
  | TRANSACTION expression

index_hint_list:
    index_hint [, index_hint] ...

index_hint:
    USE {INDEX|KEY}
      [{FOR {JOIN|ORDER BY|GROUP BY}] ([index_list])
  | IGNORE {INDEX|KEY}
      [{FOR {JOIN|ORDER BY|GROUP BY}] (index_list)
  | FORCE {INDEX|KEY}
      [{FOR {JOIN|ORDER BY|GROUP BY}] (index_list)

index_list:
    index_name [, index_name] ...
```

A table reference is also known as a join expression.

Each table can also be specified as `db_name`.`tabl_name`. This allows to write queries which involve multiple databases. See [Identifier Qualifiers](/sql-statements-structure/sql-language-structure/identifier-qualifiers/) for syntax details.

The syntax of <code class="fixed" style="white-space:pre-wrap">table_factor</code> is extended in comparison with the
SQL Standard. The latter accepts only <code class="fixed" style="white-space:pre-wrap">table_reference</code>, not a
list of them inside a pair of parentheses.

This is a conservative extension if we consider each comma in a list of
table_reference items as equivalent to an inner join. For example:

```sql
SELECT * FROM t1 LEFT JOIN (t2, t3, t4)
                 ON (t2.a=t1.a AND t3.b=t1.b AND t4.c=t1.c)
```

is equivalent to:

```sql
SELECT * FROM t1 LEFT JOIN (t2 CROSS JOIN t3 CROSS JOIN t4)
                 ON (t2.a=t1.a AND t3.b=t1.b AND t4.c=t1.c)
```

In MariaDB, <code class="fixed" style="white-space:pre-wrap">CROSS JOIN</code> is a syntactic equivalent to
<code class="fixed" style="white-space:pre-wrap">INNER JOIN</code> (they can replace each other). In standard SQL,
they are not equivalent. <code class="fixed" style="white-space:pre-wrap">INNER JOIN</code> is used with an
<code class="fixed" style="white-space:pre-wrap">ON</code> clause, <code class="fixed" style="white-space:pre-wrap">CROSS JOIN</code> is used otherwise.

In general, parentheses can be ignored in join expressions containing only
inner join operations. MariaDB also supports nested joins (see
[http://dev.mysql.com/doc/refman/5.1/en/nested-join-optimization.html](http://dev.mysql.com/doc/refman/5.1/en/nested-join-optimization.html)).

See [System-versioned tables](/sql-statements-structure/temporal-tables/system-versioned-tables/) for more information
about `FOR SYSTEM_TIME` syntax.

Index hints can be specified to affect how the MariaDB optimizer makes
use of indexes. For more information, see [How to force query plans](/kb/en/how-to-force-query-plans/).

## Examples

```sql
SELECT left_tbl.*
  FROM left_tbl LEFT JOIN right_tbl ON left_tbl.id = right_tbl.id
  WHERE right_tbl.id IS NULL;
```