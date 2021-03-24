# MIN/MAX optimization

## Min/Max optimization without GROUP BY

MariaDB and MySQL can optimize the [MIN()](/built-in-functions/aggregate-functions/min/) and [MAX()](/built-in-functions/aggregate-functions/max/) functions to be a single row lookup in the following cases:

- There is only one table used in the <code class="fixed" style="white-space:pre-wrap">SELECT</code>.
- You only have constants, <code class="fixed" style="white-space:pre-wrap">MIN()</code> and <code class="fixed" style="white-space:pre-wrap">MAX()</code> in the <code class="fixed" style="white-space:pre-wrap">SELECT</code> part.
- The argument to <code class="fixed" style="white-space:pre-wrap">MIN()</code> and <code class="fixed" style="white-space:pre-wrap">MAX()</code> is a simple column reference that is part of a key.
- There is no <code class="fixed" style="white-space:pre-wrap">WHERE</code> clause or the <code class="fixed" style="white-space:pre-wrap">WHERE</code> is used with a constant for all prefix parts of the key before the argument to <code class="fixed" style="white-space:pre-wrap">MIN()</code>/<code class="fixed" style="white-space:pre-wrap">MAX()</code>.
- If the argument is used in the <code class="fixed" style="white-space:pre-wrap">WHERE</code> clause, it can be be compared to a constant with `&lt;` or `&lt;=` in case of <code class="fixed" style="white-space:pre-wrap">MAX()</code> and with `&gt;` or `&gt;=` in case of <code class="fixed" style="white-space:pre-wrap">MIN()</code>.

Here are some examples to clarify this.
In this case we assume there is an index on columns <code class="fixed" style="white-space:pre-wrap">(a,b,c)</code>

```sql
SELECT MIN(a),MAX(a) from t1
SELECT MIN(b) FROM t1 WHERE a=const
SELECT MIN(b),MAX(b) FROM t1 WHERE a=const
SELECT MAX(c) FROM t1 WHERE a=const AND b=const
SELECT MAX(b) FROM t1 WHERE a=const AND b<const
SELECT MIN(b) FROM t1 WHERE a=const AND b>const
SELECT MIN(b) FROM t1 WHERE a=const AND b BETWEEN const AND const
SELECT MAX(b) FROM t1 WHERE a=const AND b BETWEEN const AND const
```

- Instead of `a=const` the condition `a IS NULL` can be used.

The above optimization also works for [subqueries](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/joins-subqueries/subqueries/):

```sql
SELECT x from t2 where y= (SELECT MIN(b) FROM t1 WHERE a=const)
```

Cross joins, where there is no join condition for a table, can also be optimized to a few key lookups:

```sql
select min(t1.key_part_1), max(t2.key_part_1) from t1, t2
```

## Min/Max optimization with GROUP BY

MariaDB and MySQL support loose index scan, which can speed up certain <code class="fixed" style="white-space:pre-wrap">GROUP BY</code> queries. The basic idea is that when scanning a `BTREE` index (the most common index type for the MariaDB storage engines) we can jump over identical values for any prefix of a key and thus speed up the scan significantly.

Loose scan is possible in the following cases:

- The query uses only one table.
- The <code class="fixed" style="white-space:pre-wrap">GROUP BY</code> part only uses indexed columns in the same order as in the index.
- The only aggregated functions in the <code class="fixed" style="white-space:pre-wrap">SELECT</code> part are <code class="fixed" style="white-space:pre-wrap">MIN()</code> and <code class="fixed" style="white-space:pre-wrap">MAX()</code> functions and all of them using the same column which is the next index part after the used <code class="fixed" style="white-space:pre-wrap">GROUP BY</code> columns.
- Partial indexed columns cannot be used (like only indexing 10 characters of a <code class="fixed" style="white-space:pre-wrap">VARCHAR(20)</code> column).

Loose scan will apply for your query if [<code class="fixed" style="white-space:pre-wrap">EXPLAIN</code>](/sql-statements-structure/sql-statements/administrative-sql-statements/analyze-and-explain-statements/explain/) shows `Using index for group-by` in the `Extra` column.
In this case the optimizer will do only one extra row fetch to calculate the value for <code class="fixed" style="white-space:pre-wrap">MIN()</code> or <code class="fixed" style="white-space:pre-wrap">MAX()</code> for every unique key prefix.

The following examples assume that the table `t1` has an index on `(a,b,c)`.

```sql
SELECT a, b, MIN(c),MAX(c) FROM t1 GROUP BY a,b
```

## See also

- [MIN()](/built-in-functions/aggregate-functions/min/)
- [MAX()](/built-in-functions/aggregate-functions/max/)
- [MySQL manual on loose index scans](http://dev.mysql.com/doc/refman/5.7/en/group-by-optimization.html)