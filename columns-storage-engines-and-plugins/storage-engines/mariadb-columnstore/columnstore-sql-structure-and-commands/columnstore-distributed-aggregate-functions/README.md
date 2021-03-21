# ColumnStore Distributed Aggregate Functions

MariaDB ColumnStore supports the following aggregate functions, these can be used in the SELECT, HAVING, and ORDER BY clauses of the SQL statement.

<table><tbody><tr><th>Function</th><th>Description</th></tr>
<tr><td>AVG([DISTINCT] column)</td><td>Average value of a numeric (INT variations, NUMERIC, DECIMAL) column</td></tr>
<tr><td>CORR(ColumnY, ColumnX)</td><td>The correlation coefficient for non-null pairs in a group.</td></tr>
<tr><td>COUNT (*, [DISTINCT] column)</td><td>The number of rows returned by a query or grouping. All datatypes are supported</td></tr>
<tr><td>COVAR_POP(ColumnY, ColumnX)</td><td>The population covariance for non-null pairs in a group.</td></tr>
<tr><td>COVAR_SAMP(ColumnY, ColumnX)</td><td>The sample covariance for non-null pairs in a group.</td></tr>
<tr><td>MAX ([DISTINCT] column)</td><td>The maximum value of a column. All datatypes are supported.</td></tr>
<tr><td>MIN ([DISTINCT] column)</td><td>The maximum value of a column. All datatypes are supported.</td></tr>
<tr><td>REGR_AVGX(ColumnY, ColumnX)</td><td>Average of the independent variable (sum(ColumnX)/N), where N is number of rows processed by the query</td></tr>
<tr><td>REGR_AVGY(ColumnY, ColumnX)</td><td>Average of the dependent variable (sum(ColumnY)/N), where N is number of rows processed by the query</td></tr>
<tr><td>REGR_COUNT(ColumnY, ColumnX)</td><td>The total number of input rows in which both column Y and column X are nonnull</td></tr>
<tr><td>REGR_INTERCEPT(ColumnY, ColumnX)</td><td>The y-intercept of the least-squares-fit linear equation determined by the (ColumnX, ColumnY) pairs</td></tr>
<tr><td>REGR_R2(ColumnY, ColumnX)</td><td>Square of the correlation coefficient. correlation coefficient is the regr_intercept(ColumnY, ColumnX) for linear model</td></tr>
<tr><td>REGR_SLOPE(ColumnY, ColumnX)</td><td>The slope of the least-squares-fit linear equation determined by the (ColumnX, ColumnY) pairs</td></tr>
<tr><td>REGR_SXX(ColumnY, ColumnX)</td><td>REGR_COUNT(y, x) * VAR_POP(x) for non-null pairs.</td></tr>
<tr><td>REGR_SXY(ColumnY, ColumnX)</td><td>REGR_COUNT(y, x) * COVAR_POP(y, x) for non-null pairs.</td></tr>
<tr><td>REGR_SYY(ColumnY, ColumnX)</td><td>REGR_COUNT(y, x) * VAR_POP(y) for non-null pairs.</td></tr>
<tr><td>STD(), STDDEV(), STDDEV_POP()</td><td>The population standard deviation of a numeric (INT variations, NUMERIC, DECIMAL) column</td></tr>
<tr><td>STDDEV_SAMP()</td><td>The sample standard deviation of a numeric (INT variations, NUMERIC, DECIMAL) column</td></tr>
<tr><td>SUM([DISTINCT] column)</td><td>The sum of a numeric (INT variations, NUMERIC, DECIMAL) column</td></tr>
<tr><td>VARIANCE(), VAR_POP()</td><td>The population standard variance of a numeric (INT variations, NUMERIC, DECIMAL) column</td></tr>
<tr><td>VAR_SAMP()</td><td>The population standard variance of a numeric (INT variations, NUMERIC, DECIMAL) column</td></tr>
</tbody></table>

### Note

- Regression functions (REGR_AVGX to REGR_YY), CORR, COVAR_POP and COVAR_SAMP are supported for version 1.2.0 and higher

### Example

An example group by query using aggregate functions is:

```sql
select year(o_orderdate) order_year, 
avg(o_totalprice) avg_totalprice, 
max(o_totalprice) max_totalprice, 
count(*) order_count 
from orders 
group by order_year 
order by order_year;
```