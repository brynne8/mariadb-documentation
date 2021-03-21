# ColumnStore Window Functions

# Introduction

MariaDB ColumnStore provides support for window functions broadly following the SQL 2003 specification. A window function allows for calculations relating to a window of data surrounding the current row in a result set. This capability provides for simplified queries in support of common business questions such as cumulative totals, rolling averages, and top 10 lists.

Aggregate functions are utilized for window functions however differ in behavior from a group by query because the rows remain ungrouped. This provides support for cumulative  sums and rolling averages, for example.

Two key concepts for window functions are Partition and Frame:

- A Partition is a group of rows, or window, that have the same value for a specific column, for example a Partition can be created over a time period such as a quarter or lookup values.
- The Frame for each row  is a subset of the row's Partition. The frame typically is dynamic allowing for a sliding frame of rows within the Partition. The Frame determines the range of rows for the windowing function. A Frame could be defined as the last X rows and next Y rows all the way up to the entire Partition.

Window functions are applied after joins, group by, and having clauses are calculated.

# Syntax

A window function is applied in the select clause using the following syntax:

```sql
function_name ([expression [, expression ... ]]) OVER ( window_definition )
```

where <em>window_definition</em> is defined as:

```sql
[ PARTITION BY expression [, ...] ]
[ ORDER BY expression [ ASC | DESC ] [ NULLS { FIRST | LAST } ] [, ...] ]
[ frame_clause ]
```

PARTITION BY:

- Divides the window result set into groups based on one or more <em>expressions</em>.
- An expression may be a constant, column, and non window function expressions.
- A query is not limited to a single partition by clause. Different partition clauses can be used across different window function applications.
- The partition by columns do not need to be in the select list but do need to be available from the query result set.
- If there is no PARTITION BY clause, all rows of the result set define the group.

ORDER BY

- Defines the ordering of values within the partition.
- Can be ordered by multiple keys which may be a constant, column or non window function expression.
- The order by columns do not need to be in the select list but need to be available from the query result set.
- Use of a select column alias from the query is not supported.
- ASC (default) and DESC options allow for ordering ascending or descending.
- NULLS FIRST and NULL_LAST options specify whether null values come first or last in the ordering sequence. NULLS_FIRST is the default for ASC order, and NULLS_LAST is the default for DESC order.

and the optional <em>frame_clause</em> is defined as:

```sql
{ RANGE | ROWS } frame_start
{ RANGE | ROWS } BETWEEN frame_start AND frame_end
```

and the optional <em>frame_start</em> and <em>frame_end</em> are defined as (value being a numeric expression):

```sql
UNBOUNDED PRECEDING
value PRECEDING
CURRENT ROW
value FOLLOWING
UNBOUNDED FOLLOWING
```

RANGE/ROWS:

- Defines the windowing clause for calculating the set of rows that the function applies to for calculating a given rows window function result.
- Requires an ORDER BY clause to define the row order for the window.
- ROWS specify the window in physical units, i.e. result set rows and must be a constant or expression evaluating to a positive numeric value.
- RANGE specifies the window as a logical offset. If the the expression evaluates to a numeric value then the ORDER BY expression must be a numeric or DATE type. If the expression evaluates to an interval value then the ORDER BY expression must be a DATE data type.
- UNBOUNDED PRECEDING indicates the window starts at the first row of the partition.
- UNBOUNDED FOLLOWING indicates the window ends at the last row of the partition.
- CURRENT ROW specifies the window start or ends at the current row or value.
- If omitted, the default is ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW.

# Supported Functions

<table><tbody><tr><th>Function</th><th>Description</th></tr>
<tr><td>AVG()</td><td>The average of all input values.</td></tr>
<tr><td>CORR(ColumnY, ColumnX)</td><td>The correlation coefficient for non-null pairs in a group.</td></tr>
<tr><td>COUNT()</td><td>Number of input rows.</td></tr>
<tr><td>COVAR_POP(ColumnY, ColumnX)</td><td>The population covariance for non-null pairs in a group.</td></tr>
<tr><td>COVAR_SAMP(ColumnY, ColumnX)</td><td>The sample covariance for non-null pairs in a group.</td></tr>
<tr><td>CUME_DIST()</td><td>Calculates the cumulative distribution, or relative rank, of the current row to other rows in the same partition. Number of peer or preceding rows / number of rows in partition.</td></tr>
<tr><td>DENSE_RANK()</td><td>Ranks items in a group leaving no gaps in ranking sequence when there are ties.</td></tr>
<tr><td>FIRST_VALUE()</td><td>The value evaluated at the row that is the first row of the window frame (counting from 1); null if no such row.</td></tr>
<tr><td>LAG()</td><td>The value evaluated at the row that is offset rows before the current row within the partition; if there is no such row, instead return default. Both offset and default are evaluated with respect to the current row. If omitted, offset defaults to 1 and default to null. LAG provides access to more than one row of a table at the same time without a self-join. Given a series of rows returned from a query and a position of the cursor, LAG provides access to a row at a given physical offset prior to that position.</td></tr>
<tr><td>LAST_VALUE()</td><td>The value evaluated at the row that is the last row of the window frame (counting from 1); null if no such row.</td></tr>
<tr><td>LEAD()</td><td>Provides access to a row at a given physical offset beyond that position. Returns value evaluated at the row that is offset rows after the current row within the partition; if there is no such row, instead return default. Both offset and default are evaluated with respect to the current row. If omitted, offset defaults to 1 and default to null.</td></tr>
<tr><td>MAX()</td><td>Maximum value of expression across all input values.</td></tr>
<tr><td>MEDIAN()</td><td>An inverse distribution function that assumes a continuous distribution model. It takes a numeric or datetime value and returns the middle value or an interpolated value that would be the middle value once the values are sorted. Nulls are ignored in the calculation.<strong> Not available in MariaDB Columnstore 1.1 </strong></td></tr>
<tr><td>MIN()</td><td>Minimum value of expression across all input values.</td></tr>
<tr><td>NTH_VALUE()</td><td>The value evaluated at the row that is the nth row of the window frame (counting from 1); null if no such row.</td></tr>
<tr><td>NTILE()</td><td>Divides an ordered data set into a number of buckets indicated by expr and assigns the appropriate bucket number to each row. The buckets are numbered 1 through expr. The expr value must resolve to a positive constant for each partition. Integer ranging from 1 to the argument value, dividing the partition as equally as possible.</td></tr>
<tr><td>PERCENT_RANK()</td><td>Relative rank of the current row: (rank - 1) / (total rows - 1).</td></tr>
<tr><td>PERCENTILE_CONT()</td><td>An inverse distribution function that assumes a continuous distribution model. It takes a percentile value and a sort specification, and returns an interpolated value that would fall into that percentile value with respect to the sort specification. Nulls are ignored in the calculation. <strong> Not available in MariaDB Columnstore 1.1 </strong></td></tr>
<tr><td>PERCENTILE_DISC()</td><td>An inverse distribution function that assumes a discrete distribution model. It takes a percentile value and a sort specification and returns an element from the set. Nulls are ignored in the calculation. <strong> Not available in MariaDB Columnstore 1.1 </strong></td></tr>
<tr><td>RANK()</td><td>Rank of the current row with gaps; same as row_number of its first peer.</td></tr>
<tr><td>REGR_AVGX(ColumnY, ColumnX)</td><td>Average of the independent variable (sum(ColumnX)/N), where N is number of rows processed by the query</td></tr>
<tr><td>REGR_AVGY(ColumnY, ColumnX)</td><td>Average of the dependent variable (sum(ColumnY)/N), where N is number of rows processed by the query</td></tr>
<tr><td>REGR_COUNT(ColumnY, ColumnX)</td><td>The total number of input rows in which both column Y and column X are nonnull</td></tr>
<tr><td>REGR_SLOPE(ColumnY, ColumnX)</td><td>The slope of the least-squares-fit linear equation determined by the (ColumnX, ColumnY) pairs</td></tr>
<tr><td>REGR_INTERCEPT(ColumnY, ColumnX)</td><td>The y-intercept of the least-squares-fit linear equation determined by the (ColumnX, ColumnY) pairs</td></tr>
<tr><td>REGR_R2(ColumnY, ColumnX)</td><td>Square of the correlation coefficient. correlation coefficient is the regr_intercept(ColumnY, ColumnX) for linear model</td></tr>
<tr><td>REGR_SXX(ColumnY, ColumnX)</td><td>REGR_COUNT(y, x) * VAR_POP(x) for non-null pairs.</td></tr>
<tr><td>REGR_SXY(ColumnY, ColumnX)</td><td>REGR_COUNT(y, x) * COVAR_POP(y, x) for non-null pairs.</td></tr>
<tr><td>REGR_SYY(ColumnY, ColumnX)</td><td>REGR_COUNT(y, x) * VAR_POP(y) for non-null pairs.</td></tr>
<tr><td>ROW_NUMBER()</td><td>Number of the current row within its partition, counting from 1</td></tr>
<tr><td>STDDEV() STDDEV_POP()</td><td>Computes the population standard deviation and returns the square root of the population variance.</td></tr>
<tr><td>STDDEV_SAMP()</td><td>Computes the cumulative sample standard deviation and returns the square root of the sample variance.</td></tr>
<tr><td>SUM()</td><td>Sum of expression across all input values.</td></tr>
<tr><td>VARIANCE() VAR_POP()</td><td>Population variance of the input values (square of the population standard deviation).</td></tr>
<tr><td>VAR_SAMP()</td><td>Sample variance of the input values (square of the sample standard deviation).</td></tr>
</tbody></table>

### Note

- Regression functions (REGR_AVGX to REGR_YY), CORR, COVAR_POP and COVAR_SAMP are supported for version 1.2.0 and higher

# Examples

## Example Schema

The examples are all based on the following simplified sales opportunity table:

```sql
create table opportunities (
id int,
accountName varchar(20),
name varchar(128),
owner varchar(7),
amount decimal(10,2),
closeDate date,
stageName varchar(11)
) engine=columnstore;
```

Some example values are (thanks to [https://www.mockaroo.com](https://www.mockaroo.com) for sample data generation):

<table><tbody><tr><th>id</th><th>accountName</th><th>name</th><th>owner</th><th>amount</th><th>closeDate</th><th>stageName</th></tr>
<tr><td>1</td><td>Browseblab</td><td>Multi-lateral executive function</td><td>Bob</td><td>26444.86</td><td>2016-10-20</td><td>Negotiating</td></tr>
<tr><td>2</td><td>Mita</td><td>Organic demand-driven benchmark</td><td>Maria</td><td>477878.41</td><td>2016-11-28</td><td>ClosedWon</td></tr>
<tr><td>3</td><td>Miboo</td><td>De-engineered hybrid groupware</td><td>Olivier</td><td>80181.78</td><td>2017-01-05</td><td>ClosedWon</td></tr>
<tr><td>4</td><td>Youbridge</td><td>Enterprise-wide bottom-line Graphic Interface</td><td>Chris</td><td>946245.29</td><td>2016-07-02</td><td>ClosedWon</td></tr>
<tr><td>5</td><td>Skyba</td><td>Reverse-engineered fresh-thinking standardization</td><td>Maria</td><td>696241.82</td><td>2017-02-17</td><td>Negotiating</td></tr>
<tr><td>6</td><td>Eayo</td><td>Fundamental well-modulated artificial intelligence</td><td>Bob</td><td>765605.52</td><td>2016-08-27</td><td>Prospecting</td></tr>
<tr><td>7</td><td>Yotz</td><td>Extended secondary infrastructure</td><td>Chris</td><td>319624.20</td><td>2017-01-06</td><td>ClosedLost</td></tr>
<tr><td>8</td><td>Oloo</td><td>Configurable web-enabled data-warehouse</td><td>Chris</td><td>321016.26</td><td>2017-03-08</td><td>ClosedLost</td></tr>
<tr><td>9</td><td>Kaymbo</td><td>Multi-lateral web-enabled definition</td><td>Bob</td><td>690881.01</td><td>2017-01-02</td><td>Developing</td></tr>
<tr><td>10</td><td>Rhyloo</td><td>Public-key coherent infrastructure</td><td>Chris</td><td>965477.74</td><td>2016-11-07</td><td>Prospecting</td></tr>
</tbody></table>

The schema, sample data, and queries are available as an attachment to this article.

## Cumulative Sum and Running Max Example

Window functions can be used to achieve cumulative / running calculations on a detail report. In this case a won opportunity report for a 7 day period adds columns to show the accumulated won amount as well as the current highest opportunity amount in preceding rows.

```sql
select owner, 
accountName, 
CloseDate, 
amount, 
sum(amount) over (order by CloseDate rows between unbounded preceding and current row) cumeWon, 
max(amount) over (order by CloseDate rows between unbounded preceding and current row) runningMax
from opportunities 
where stageName='ClosedWon' 
and closeDate >= '2016-10-02' and closeDate <= '2016-10-09' 
order by CloseDate;
```

with example results:

<table><tbody><tr><th>owner</th><th>accountName</th><th>CloseDate</th><th>amount</th><th>cumeWon</th><th>runningMax</th></tr>
<tr><td>Bill</td><td>Babbleopia</td><td>2016-10-02</td><td>437636.47</td><td>437636.47</td><td>437636.47</td></tr>
<tr><td>Bill</td><td>Thoughtworks</td><td>2016-10-04</td><td>146086.51</td><td>583722.98</td><td>437636.47</td></tr>
<tr><td>Olivier</td><td>Devpulse</td><td>2016-10-05</td><td>834235.93</td><td>1417958.91</td><td>834235.93</td></tr>
<tr><td>Chris</td><td>Linkbridge</td><td>2016-10-07</td><td>539977.45</td><td>2458738.65</td><td>834235.93</td></tr>
<tr><td>Olivier</td><td>Trupe</td><td>2016-10-07</td><td>500802.29</td><td>1918761.20</td><td>834235.93</td></tr>
<tr><td>Bill</td><td>Latz</td><td>2016-10-08</td><td>857254.87</td><td>3315993.52</td><td>857254.87</td></tr>
<tr><td>Chris</td><td>Avamm</td><td>2016-10-09</td><td>699566.86</td><td>4015560.38</td><td>857254.87</td></tr>
</tbody></table>

## Partitioned Cumulative Sum and Running Max Example

The above example can be partitioned, so that the window functions are over a particular field grouping such as owner and accumulate within that grouping. This is achieved by adding the syntax "partition by &lt;columns&gt;" in the window function clause.

```sql
select owner,  
accountName,  
CloseDate,  
amount,  
sum(amount) over (partition by owner order by CloseDate rows between unbounded preceding and current row) cumeWon,  
max(amount) over (partition by owner order by CloseDate rows between unbounded preceding and current row) runningMax 
from opportunities  
where stageName='ClosedWon' 
and closeDate >= '2016-10-02' and closeDate <= '2016-10-09'  
order by owner, CloseDate;
```

with example results:

<table><tbody><tr><th>owner</th><th>accountName</th><th>CloseDate</th><th>amount</th><th>cumeWon</th><th>runningMax</th></tr>
<tr><td>Bill</td><td>Babbleopia</td><td>2016-10-02</td><td>437636.47</td><td>437636.47</td><td>437636.47</td></tr>
<tr><td>Bill</td><td>Thoughtworks</td><td>2016-10-04</td><td>146086.51</td><td>583722.98</td><td>437636.47</td></tr>
<tr><td>Bill</td><td>Latz</td><td>2016-10-08</td><td>857254.87</td><td>1440977.85</td><td>857254.87</td></tr>
<tr><td>Chris</td><td>Linkbridge</td><td>2016-10-07</td><td>539977.45</td><td>539977.45</td><td>539977.45</td></tr>
<tr><td>Chris</td><td>Avamm</td><td>2016-10-09</td><td>699566.86</td><td>1239544.31</td><td>699566.86</td></tr>
<tr><td>Olivier</td><td>Devpulse</td><td>2016-10-05</td><td>834235.93</td><td>834235.93</td><td>834235.93</td></tr>
<tr><td>Olivier</td><td>Trupe</td><td>2016-10-07</td><td>500802.29</td><td>1335038.22</td><td>834235.93</td></tr>
</tbody></table>

## Ranking / Top Results

The rank window function allows for ranking or assigning a numeric order value based on the window function definition.  Using  the Rank() function will result in the same value for ties / equal values and the next rank value skipped. The Dense_Rank() function behaves similarly except the next consecutive number is used after a tie rather than skipped. The Row_Number() function will provide a unique ordering value. The example query shows the Rank() function being applied to rank sales reps by the number of opportunities for Q4 2016.

```sql
select owner, 
wonCount, 
rank() over (order by wonCount desc) rank 
from (
  select owner, 
  count(*) wonCount 
  from opportunities 
  where stageName='ClosedWon' 
  and closeDate >= '2016-10-01' and closeDate < '2016-12-31'  
  group by owner
) t
order by rank;
```

with example results (note the query is technically incorrect by using closeDate &lt; '2016-12-31' however this creates a tie scenario for illustrative purposes):

<table><tbody><tr><th>owner</th><th>wonCount</th><th>rank</th></tr>
<tr><td>Bill</td><td>19</td><td>1</td></tr>
<tr><td>Chris</td><td>15</td><td>2</td></tr>
<tr><td>Maria</td><td>14</td><td>3</td></tr>
<tr><td>Bob</td><td>14</td><td>3</td></tr>
<tr><td>Olivier</td><td>10</td><td>5</td></tr>
</tbody></table>

If the dense_rank function is used the rank values would be 1,2,3,3,4 and for the row_number function the values would be 1,2,3,4,5.

## First and Last Values

The first_value and last_value functions allow determining the first and last values of a given range. Combined with a group by this allows summarizing opening and closing values. The example shows a more complex case where detailed information is presented for first and last opportunity by quarter.

```sql
select a.year, 
a.quarter, 
f.accountName firstAccountName, 
f.owner firstOwner, 
f.amount firstAmount, 
l.accountName lastAccountName, 
l.owner lastOwner, 
l.amount lastAmount 
from (
  select year, 
  quarter, 
  min(firstId) firstId, 
  min(lastId) lastId 
  from (
    select year(closeDate) year, 
    quarter(closeDate) quarter, 
    first_value(id) over (partition by year(closeDate), quarter(closeDate) order by closeDate rows between unbounded preceding and current row) firstId, 
    last_value(id) over (partition by year(closeDate), quarter(closeDate) order by closeDate rows between current row and unbounded following) lastId 
    from opportunities  where stageName='ClosedWon'
  ) t 
  group by year, quarter order by year,quarter
) a 
join opportunities f on a.firstId = f.id 
join opportunities l on a.lastId = l.id 
order by year, quarter;
```

with example results:

<table><tbody><tr><th>year</th><th>quarter</th><th>firstAccountName</th><th>firstOwner</th><th>firstAmount</th><th>lastAccountName</th><th>lastOwner</th><th>lastAmount</th></tr>
<tr><td>2016</td><td>3</td><td>Skidoo</td><td>Bill</td><td>523295.07</td><td>Skipstorm</td><td>Bill</td><td>151420.86</td></tr>
<tr><td>2016</td><td>4</td><td>Skimia</td><td>Chris</td><td>961513.59</td><td>Avamm</td><td>Maria</td><td>112493.65</td></tr>
<tr><td>2017</td><td>1</td><td>Yombu</td><td>Bob</td><td>536875.51</td><td>Skaboo</td><td>Chris</td><td>270273.08</td></tr>
</tbody></table>

## Prior and Next Example

Sometimes it useful to understand the previous and next values in the context of a given row. The lag and lead window functions provide this capability. By default the offset is one providing the prior or next value but can also be provided to get a larger offset. The example query is a report of opportunities by account name showing the opportunity amount, and the prior and next opportunity amount for that account by close date.

```sql
select accountName, 
closeDate,  
amount currentOppAmount, 
lag(amount) over (partition by accountName order by closeDate) priorAmount, lead(amount) over (partition by accountName order by closeDate) nextAmount 
from opportunities 
order by accountName, closeDate 
limit 9;
```

with example results:

<table><tbody><tr><th>accountName</th><th>closeDate</th><th>currentOppAmount</th><th>priorAmount</th><th>nextAmount</th></tr>
<tr><td>Abata</td><td>2016-09-10</td><td>645098.45</td><td>NULL</td><td>161086.82</td></tr>
<tr><td>Abata</td><td>2016-10-14</td><td>161086.82</td><td>645098.45</td><td>350235.75</td></tr>
<tr><td>Abata</td><td>2016-12-18</td><td>350235.75</td><td>161086.82</td><td>878595.89</td></tr>
<tr><td>Abata</td><td>2016-12-31</td><td>878595.89</td><td>350235.75</td><td>922322.39</td></tr>
<tr><td>Abata</td><td>2017-01-21</td><td>922322.39</td><td>878595.89</td><td>NULL</td></tr>
<tr><td>Abatz</td><td>2016-10-19</td><td>795424.15</td><td>NULL</td><td>NULL</td></tr>
<tr><td>Agimba</td><td>2016-07-09</td><td>288974.84</td><td>NULL</td><td>914461.49</td></tr>
<tr><td>Agimba</td><td>2016-09-07</td><td>914461.49</td><td>288974.84</td><td>176645.52</td></tr>
<tr><td>Agimba</td><td>2016-09-20</td><td>176645.52</td><td>914461.49</td><td>NULL</td></tr>
</tbody></table>

## Quartiles Example

The NTile window function allows for breaking up a data set into portions assigned a numeric value to each portion of the range. NTile(4) breaks the data up into quartiles (4 sets). The example query produces a report of all opportunities summarizing the quartile boundaries of amount values.

```sql
select t.quartile, 
min(t.amount) min, 
max(t.amount) max 
from (
  select amount, 
  ntile(4) over (order by amount asc) quartile 
  from opportunities 
  where closeDate >= '2016-10-01' and closeDate <= '2016-12-31'
  ) t 
group by quartile 
order by quartile;
```

With example results:

<table><tbody><tr><th>quartile</th><th>min</th><th>max</th></tr>
<tr><td>1</td><td>6337.15</td><td>287634.01</td></tr>
<tr><td>2</td><td>288796.14</td><td>539977.45</td></tr>
<tr><td>3</td><td>540070.04</td><td>748727.51</td></tr>
<tr><td>4</td><td>753670.77</td><td>998864.47</td></tr>
</tbody></table>

## Percentile Example

The percentile functions have a slightly different syntax from other window functions as can be seen in the example below. These functions can be only applied against numeric values.  The argument to the function is the percentile to evaluate. Following 'within group' is the sort expression which indicates the sort column and optionally order. Finally after 'over' is an optional partition by clause, for no partition clause use 'over ()'. The example below utilizes the value 0.5 to calculate the median opportunity amount in the rows. The values differ sometimes because percentile_cont will return the average of the 2 middle rows for an even data set while percentile_desc returns the first encountered in the sort.

<strong> Note that the percentile syntax is supported in MariaDB ColumnStore 1.0 but not 1.1. It is anticipated that this will be restored in MariaDB ColumnStore 1.2 after this is implemented in MariaDB server 10.3.</strong>

```sql
select owner,  
accountName,  
CloseDate,  
amount,
percentile_cont(0.5) within group (order by amount) over (partition by owner) pct_cont,
percentile_disc(0.5) within group (order by amount) over (partition by owner) pct_disc
from opportunities  
where stageName='ClosedWon' 
and closeDate >= '2016-10-02' and closeDate <= '2016-10-09'  
order by owner, CloseDate;
```

With example results:

<table><tbody><tr><th>owner</th><th>accountName</th><th>CloseDate</th><th>amount</th><th>pct_cont</th><th>pct_disc</th></tr>
<tr><td>Bill</td><td>Babbleopia</td><td>2016-10-02</td><td>437636.47</td><td>437636.4700000000</td><td>437636.47</td></tr>
<tr><td>Bill</td><td>Thoughtworks</td><td>2016-10-04</td><td>146086.51</td><td>437636.4700000000</td><td>437636.47</td></tr>
<tr><td>Bill</td><td>Latz</td><td>2016-10-08</td><td>857254.87</td><td>437636.4700000000</td><td>437636.47</td></tr>
<tr><td>Chris</td><td>Linkbridge</td><td>2016-10-07</td><td>539977.45</td><td>619772.1550000000</td><td>539977.45</td></tr>
<tr><td>Chris</td><td>Avamm</td><td>2016-10-09</td><td>699566.86</td><td>619772.1550000000</td><td>539977.45</td></tr>
<tr><td>Olivier</td><td>Devpulse</td><td>2016-10-05</td><td>834235.93</td><td>667519.1100000000</td><td>500802.29</td></tr>
<tr><td>Olivier</td><td>Trupe</td><td>2016-10-07</td><td>500802.29</td><td>667519.1100000000</td><td>500802.29</td></tr>
</tbody></table>