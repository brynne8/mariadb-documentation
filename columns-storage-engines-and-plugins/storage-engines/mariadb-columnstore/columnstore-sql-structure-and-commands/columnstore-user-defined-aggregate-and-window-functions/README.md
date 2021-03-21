# ColumnStore User Defined Aggregate and Window Functions

## Introduction

Starting with MariaDB ColumnStore 1.1, the ability to create and use user defined aggregate and window functions is supported in addition to scalar functions. With Columnstore 1.2, multiple parameters are supported.  A C++ SDK is provided as well as 3 reference examples that provide additional functions that may be of general use:

- <strong>median</strong> - mathematical median, equivalent to percentile_cont(0.5)
- <strong>avg_mode</strong> - mathematical mode, i.e the most frequent value in the set
- <strong>ssq</strong> - sum of squares, i.e the sum of each individual number squared in the set

Similar to built in functions, the SDK supports distributed aggregate execution where as much of the  calculation is scaled out across PM nodes and then collected / finalized in the UM node. Window functions (due to the ordering requirement) are only executed at the UM level.

## Using user defined aggregate functions

The reference examples above are included in the standard build of MariaDB ColumnStore and so can be used by registering them as user defined aggregate functions. The same can be done for new functions assuming the instance has the updated libraries included. From a mcsmysql prompt:

```sql
CREATE AGGREGATE FUNCTION median returns REAL soname 'libudf_mysql.so';
CREATE AGGREGATE FUNCTION avg_mode returns REAL soname 'libudf_mysql.so';
CREATE AGGREGATE FUNCTION ssq returns REAL soname 'libudf_mysql.so';
```

After this these may be used in the same way as any other aggregate or window function like sum:

```sql
SELECT grade, 
AVG(loan_amnt) avg, 
MEDIAN(loan_amnt) median 
FROM loanstats 
GROUP BY grade 
ORDER BY grade;
```

## Developing a new function

This requires a MariaDB ColumnStore source tree and necessary tools to compile C/C++ code. The SDK and reference examples are available in the [utils/udfsdk](https://github.com/mariadb-corporation/mariadb-columnstore-engine/tree/master/utils/udfsdk) directory of the source tree. This contains the SDK documentation which is also available here:

- [1.2.x UDAF SDK Guide](/kb/en/columnstore-user-defined-aggregate-and-window-functions/+attachment/mariadb_columnstore_udaf_sdk_1-2)

## Limitations

- The implementation of the median and avg_mode functions will scale in memory consumption to the size of the set of unique values in the aggregation.