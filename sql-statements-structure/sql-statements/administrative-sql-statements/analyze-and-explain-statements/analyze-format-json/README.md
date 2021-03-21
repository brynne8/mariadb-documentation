# ANALYZE FORMAT=JSON

`ANALYZE FORMAT=JSON` is a mix of the [EXPLAIN FORMAT=JSON](/kb/en/explain-formatjson/) and [ANALYZE](/sql-statements-structure/sql-statements/administrative-sql-statements/analyze-and-explain-statements/analyze-statement) statement features. The `ANALYZE FORMAT=JSON $statement` will execute `$statement`, and then print the output of `EXPLAIN FORMAT=JSON`, amended with data from the query execution.

## Basic Execution Data

You can get the following also from tabular `ANALYZE` statement form:

- <strong>`r_rows`</strong>  is provided for any node that reads rows. It shows how many rows were read, on average
- <strong>`r_filtered`</strong> is provided whenever there is a condition that is checked.  It shows the percentage of rows left after checking the condition.

## Advanced Execution Data

The most important data not available in the regular tabula `ANALYZE` statement are:

- <strong>`r_loops`</strong> field.  This shows how many times the node was executed. Most query plan elements have this field.
- <strong>`r_total_time_ms`</strong> field. It shows how much time in total was spent executing this node. If the node has subnodes, their execution time is included.
- <strong>`r_buffer_size`</strong> field. Query plan nodes that make use of buffers report the size of buffer that was was used.

## Data About Individual Query Plan Nodes

- <strong>`filesort`</strong> node reports whether sorting was done with `LIMIT n` parameter, and how many rows were in the sort result.
- <strong>`block-nl-join`</strong> node has `r_loops` field, which allows to tell whether `Using join buffer` was efficient
- <strong>`range-checked-for-each-record`</strong> reports counters that show the result of the check.
- <strong>`expression-cache`</strong> is used for subqueries, and it reports how many times the cache was used, and what cache hit ratio was.
- <strong>`union_result`</strong> node has `r_rows` so one can see how many rows were produced after UNION operation
- and so forth

## Use Cases

See [Examples of ANALYZE FORMAT=JSON](/sql-statements-structure/sql-statements/administrative-sql-statements/analyze-and-explain-statements/analyze-formatjson-examples).