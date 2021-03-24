# WITH

##### MariaDB starting with [10.2.1](/kb/en/mariadb-1021-release-notes/)

Common Table Expressions were introduced in [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/).

### Syntax

```sql
WITH [RECURSIVE] table_reference [(columns_list)] AS  (
  SELECT ...
)
[CYCLE cycle_column_list RESTRICT]
SELECT ...
```

### Description

The `WITH` keyword signifies a [Common Table Expression](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/common-table-expressions/) (CTE). It allows you to refer to a subquery expression many times in a query, as if having a temporary table that only exists for the duration of a query.

There are two kinds of CTEs:

- [Non-Recursive](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/common-table-expressions/non-recursive-common-table-expressions-overview/)
- [Recursive](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/common-table-expressions/recursive-common-table-expressions-overview/) (signified by the `RECURSIVE` keyword, supported since [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/))

You can use `table_reference` as any normal table in the external `SELECT` part.  You can also use `WITH` in subqueries, as well as with [EXPLAIN](/sql-statements-structure/sql-statements/administrative-sql-statements/analyze-and-explain-statements/explain/) and [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/).

Poorly-formed recursive CTEs can in theory cause infinite loops. The [max_recursive_iterations](/kb/en/server-system-variables/#max_recursive_iterations) system variable limits the number of recursions.

#### CYCLE ... RESTRICT

##### MariaDB starting with [10.5.2](/kb/en/mariadb-1052-release-notes/)

The CYCLE clause enables CTE cycle detection, avoiding excessive or infinite loops,
MariaDB supports a relaxed, non-standard grammar.

The SQL Standard permits a CYCLE clause, as follows:

```sql
WITH RECURSIVE ... (
  ...
)
CYCLE <cycle column list>
SET <cycle mark column> TO <cycle mark value> DEFAULT <non-cycle mark value>
USING <path column>
```

where all clauses are mandatory.

MariaDB does not support this, but from 10.5.2 permits a non-standard relaxed grammar, as follows:

```sql
WITH RECURSIVE ... (
  ...
)
CYCLE <cycle column list> RESTRICT
```

With the use of `CYCLE ... RESTRICT` it makes no difference whether the CTE uses `UNION ALL` or `UNION DISTINCT` anymore. `UNION ALL` means "all rows, but without cycles", which is exactly what the `CYCLE` clause enables. And `UNION DISTINCT` means all rows should be different, which, again, is what will happen — as uniqueness is enforced over a subset of columns, complete rows will automatically all be different.

### Examples

Below is an example with the `WITH` at the top level:

```sql
WITH t AS (SELECT a FROM t1 WHERE b >= 'c') 
  SELECT * FROM t2, t WHERE t2.c = t.a;
```

The example below uses `WITH` in a subquery:

```sql
SELECT t1.a, t1.b FROM t1, t2
  WHERE t1.a > t2.c 
     AND t2.c IN(WITH t AS (SELECT * FROM t1 WHERE t1.a < 5)
                SELECT t2.c FROM t2, t WHERE t2.c = t.a);
```

Below is an example of a Recursive CTE:

```sql
WITH RECURSIVE ancestors AS 
 ( SELECT * FROM folks
   WHERE name="Alex"
   UNION
   SELECT f.*
   FROM folks AS f, ancestors AS a
   WHERE f.id = a.father OR f.id = a.mother )
SELECT * FROM ancestors;
```

Take the following structure, and data,

```sql
CREATE TABLE t1 (from_ int, to_ int);
INSERT INTO t1 VALUES (1,2), (1,100), (2,3), (3,4), (4,1);
SELECT * FROM t1;
+-------+------+
| from_ | to_  |
+-------+------+
|     1 |    2 |
|     1 |  100 |
|     2 |    3 |
|     3 |    4 |
|     4 |    1 |
+-------+------+
```

Given the above, the following query would theoretically result in an infinite loop due to the last record in t1 (note that [max_recursive_iterations](/kb/en/server-system-variables/#max_recursive_iterations) is set to 10 for the purposes of this example, to avoid the excessive number of cycles):

```sql
SET max_recursive_iterations=10;

WITH RECURSIVE cte (depth, from_, to_) AS ( 
  SELECT 0,1,1 UNION DISTINCT SELECT depth+1, t1.from_, t1.to_ 
    FROM t1, cte  WHERE t1.from_ = cte.to_ 
) 
SELECT * FROM cte;
+-------+-------+------+
| depth | from_ | to_  |
+-------+-------+------+
|     0 |     1 |    1 |
|     1 |     1 |    2 |
|     1 |     1 |  100 |
|     2 |     2 |    3 |
|     3 |     3 |    4 |
|     4 |     4 |    1 |
|     5 |     1 |    2 |
|     5 |     1 |  100 |
|     6 |     2 |    3 |
|     7 |     3 |    4 |
|     8 |     4 |    1 |
|     9 |     1 |    2 |
|     9 |     1 |  100 |
|    10 |     2 |    3 |
+-------+-------+------+
```

However, the CYCLE ... RESTRICT clause (from [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/)) can overcome this:

```sql
WITH RECURSIVE cte (depth, from_, to_) AS ( 
  SELECT 0,1,1 UNION SELECT depth+1, t1.from_, t1.to_ 
    FROM t1, cte WHERE t1.from_ = cte.to_ 
) 
CYCLE from_, to_ RESTRICT 
SELECT * FROM cte;
+-------+-------+------+
| depth | from_ | to_  |
+-------+-------+------+
|     0 |     1 |    1 |
|     1 |     1 |    2 |
|     1 |     1 |  100 |
|     2 |     2 |    3 |
|     3 |     3 |    4 |
|     4 |     4 |    1 |
+-------+-------+------+
```

### See Also

- [Non-Recursive Common Table Expressions Overview](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/common-table-expressions/non-recursive-common-table-expressions-overview/)
- [Recursive Common Table Expressions Overview](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/common-table-expressions/recursive-common-table-expressions-overview/)