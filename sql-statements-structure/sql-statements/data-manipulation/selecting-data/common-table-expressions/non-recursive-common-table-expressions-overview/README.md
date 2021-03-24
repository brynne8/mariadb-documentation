# Non-Recursive Common Table Expressions Overview

Common Table Expressions (CTEs) are a standard SQL feature, and are essentially temporary named result sets. There are two kinds of CTEs: Non-Recursive, which this article covers; and [Recursive](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/common-table-expressions/recursive-common-table-expressions-overview/).

##### MariaDB starting with [10.2.1](/kb/en/mariadb-1021-release-notes/)

Common table expressions were introduced in [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/).

## Non-Recursive CTEs

The [WITH](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/common-table-expressions/with/) keyword signifies a CTE. It is given a name, followed by a body (the main query) as follows:
<img src="/kb/en/non-recursive-common-table-expressions-overview/+image/cte_syntax" alt="cte_syntax" title="cte_syntax">

CTEs are similar to derived tables. For example

```sql
WITH engineers AS 
   ( SELECT * FROM employees
     WHERE dept = 'Engineering' )

SELECT * FROM engineers
WHERE ...
```

```sql
SELECT * FROM
   ( SELECT * FROM employees
     WHERE dept = 'Engineering' ) AS engineers
WHERE
...
```

A non-recursive CTE is basically a query-local [VIEW](/programming-customizing-mariadb/views/). There are several advantages and caveats to them. The syntax is more readable than nested `FROM (SELECT ...)`.
A CTE can refer to another and it can be referenced from multiple places.

### A CTE referencing Another CTE

Using this format makes for a more readable SQL than a nested `FROM(SELECT ...)` clause.  Below is an example of this:

```sql
WITH engineers AS (
SELECT * FROM employees
WHERE dept IN('Development','Support') ),
eu_engineers AS ( SELECT * FROM engineers WHERE country IN('NL',...) )
SELECT
...
FROM eu_engineers;
```

### Multiple Uses of a CTE

This can be an 'anti-self join', for example:

```sql
WITH engineers AS (
SELECT * FROM employees
WHERE dept IN('Development','Support') )

SELECT * FROM engineers E1
WHERE NOT EXISTS
   (SELECT 1 FROM engineers E2
    WHERE E2.country=E1.country
    AND E2.name <> E1.name );
```

Or, for year-over-year comparisons, for example:

```sql
WITH sales_product_year AS (
SELECT product, YEAR(ship_date) AS year,
SUM(price) AS total_amt
FROM item_sales
GROUP BY product, year )

SELECT *
FROM sales_product_year CUR,
sales_product_year PREV,
WHERE CUR.product=PREV.product 
AND  CUR.year=PREV.year + 1 
AND CUR.total_amt > PREV.total_amt
```

Another use is to compare individuals against their group. Below is an example of how this might be executed:

```sql
WITH sales_product_year AS (
SELECT product,
YEAR(ship_date) AS year,
SUM(price) AS total_amt
FROM item_sales
GROUP BY product, year
)

SELECT * 
FROM sales_product_year S1
WHERE
total_amt > 
    (SELECT 0.1 * SUM(total_amt)
     FROM sales_product_year S2
     WHERE S2.year = S1.year)
```