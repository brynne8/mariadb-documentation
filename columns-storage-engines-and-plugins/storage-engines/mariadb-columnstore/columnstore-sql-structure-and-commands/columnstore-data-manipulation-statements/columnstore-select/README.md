# ColumnStore Select

The SELECT statement is used to query the database and display table data. You can add many clauses to filter the data.

## Syntax

```sql
SELECT
    [ALL | DISTINCT ]
    select_expr [, select_expr ...]
    [ FROM table_references
      [WHERE where_condition]
      [GROUP BY {col_name | expr | position} [ASC | DESC], ... [WITH ROLLUP]]
      [HAVING where_condition]
      [ORDER BY {col_name | expr | position} [ASC | DESC], ...]
      [LIMIT {[offset,] row_count | row_count OFFSET offset}]
      [PROCEDURE procedure_name(argument_list)]
      [INTO OUTFILE 'file_name' [CHARACTER SET charset_name] [export_options]
         | INTO DUMPFILE 'file_name' | INTO var_name [, var_name] ]
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

<code class="unknown_macro">&lt;&lt;<span class="macro_name">toc</span><span class="macro_arg_string"></span>&gt;&gt;</code>

## Projection List (SELECT)

If the same column needs to be referenced more than once in the projection list, a unique name is required for each column using a column alias.The total length of the name of a column, inclusive of length of functions, in the projection list must be 64 characters or less.

## WHERE

The WHERE clause filters data retrieval based on criteria. Note that <em>column_alias</em> cannot be used in the WHERE clause.The following statement returns rows in the region table where the region  = ‘ASIA’:

```sql
SELECT * FROM region WHERE name = ’ASIA’;
```

## GROUP BY

GROUP BY groups data based on values in one or more specific columns. A maximum of 10 columns will be supported in the GROUP BY clause.The following statement returns rows from the <em>lineitem</em> table where /orderkey<em> is less than 1 000 000 and groups them by the quantity.</em>

```sql
SELECT quantity, count(*) FROM lineitem WHERE orderkey < 1000000 GROUP BY quantity;
```

## HAVING

HAVING is used in combination with the GROUP BY clause. It can be used in a SELECT statement to filter the records that a GROUP BY returns.The following statement returns shipping dates, and the respective quantity where the quantity is 2500 or more.

```sql
SELECT shipdate, count(*) FROM lineitem GROUP BYshipdate HAVING count(*) >= 2500;
```

## ORDER BY

The ORDER BY clause presents results in a specific order. Note that the ORDER BY clause represents a statement that is post-processed by MariaDB. The following statement returns an ordered <em>quantity</em> column from the <em>lineitem</em> table.

```sql
SELECT quantity FROM lineitem WHERE orderkey < 1000000order by quantity;
```

The following statement returns an ordered <em>shipmode</em> column from the <em>lineitem</em> table.

```sql
Select shipmode from lineitem where orderkey < 1000000order by 1;
```

<strong>NOTE: When ORDER BY is used in an inner query and LIMIT on an outer query, LIMIT is applied first and then ORDER BY is applied when returning results.</strong>

## UNION

Used to combine the result from multiple SELECT statements into a single result set.The UNION or UNION DISTINCT clause returns query results from multiple queries into one display and discards duplicate results. The UNION ALL clause displays query results from multiple queries and does not discard the duplicates. The following statement returns the <em>p_name</em> rows in the <em>part</em> table and the <em>partno</em> table and discards the duplicate results:

```sql
SELECT p_name FROM part UNION select p_name FROM  partno;
```

The following statement returns all the <em>p_name rows</em> in the <em>part</em> table and the <em>partno</em> table:

```sql
SELECT p_name FROM part UNION ALL select p_name FROM  partno;
```

## LIMIT

Limit is used to constrain the number of rows returned by the SELECT statement. LIMIT can have up to two arguments. LIMIT must contain a rowcount and may optionally contain an offset of the first row to return (initial row is 0).
The following statement returns 5 customer keys from the customer table:

```sql
SELECT custkey from customer limit 5;
```

The following statement returns 5 customer keys from the customer table beginning at offset 1000:

```sql
SELECT custkey from customer limit 1000,5;
```

<strong>NOTE: When LIMIT is applied on a nested query's results, and the inner query contains ORDER BY, LIMIT is applied first and then ORDER BY is applied.(Valid for Columnstore 1.0.x - 1.2.x)</strong>