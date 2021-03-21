# Query Optimizations

Different query optimizations and how you can use and tune them to get better performance.

- [Index Hints: How to Force Query Plans](/replication/optimization-and-tuning/query-optimizations/index-hints-how-to-force-query-plans/) — Using hints to get the optimizer to use another query plan.
- [Subquery Optimizations](/replication/optimization-and-tuning/query-optimizations/subquery-optimizations/) — Articles about subquery optimizations in MariaDB.
- [Optimization Strategies](/replication/optimization-and-tuning/query-optimizations/optimization-strategies/) — Various optimization strategies used by the query optimizer.
- [Optimizations for Derived Tables](/replication/optimization-and-tuning/query-optimizations/optimizations-for-derived-tables/) — Optimizations for derived tables, or subqueries in the FROM clause
- [Table Elimination](/replication/optimization-and-tuning/query-optimizations/table-elimination/) — Resolving queries without accessing some of the tables the query refers to
- [Statistics for Optimizing Queries](/replication/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/) — Different statistics provided by MariaDB to help you optimize your queries
- [MIN/MAX optimization](/replication/optimization-and-tuning/query-optimizations/minmax-optimization/) — How MIN and MAX are optimized
- [Filesort with Small LIMIT Optimization](/replication/optimization-and-tuning/query-optimizations/filesort-with-small-limit-optimization/) — MariaDB 10's filesort with small LIMIT optimization
- [LIMIT ROWS EXAMINED](/replication/optimization-and-tuning/query-optimizations/limit-rows-examined/) — Means to terminate execution of SELECTs that examine too many rows.
- [Block-Based Join Algorithms](/replication/optimization-and-tuning/query-optimizations/block-based-join-algorithms/) — Algorithms that employ a join buffer for the first join before starting to look in the second.
- [index_merge sort_intersection](/replication/optimization-and-tuning/query-optimizations/index_merge-sort_intersection/) — Operation to allow the use of index_merge in a broader number of cases
- [MariaDB 5.3 Optimizer Debugging](/replication/optimization-and-tuning/query-optimizations/mariadb-53-optimizer-debugging/) — MariaDB 5.3's optimizer debugging patch
- [optimizer_switch](/replication/optimization-and-tuning/query-optimizations/optimizer-switch/) — Server variable for enabling specific optimizations.
- [Extended Keys](/replication/optimization-and-tuning/query-optimizations/extended-keys/) — Optimization using InnoDB/XtraDB key components to
generate more efficient execution plans.
- [How to Quickly Insert Data Into MariaDB](/replication/optimization-and-tuning/query-optimizations/how-to-quickly-insert-data-into-mariadb/) — Techniques for inserting data quickly into MariaDB.
- [Index Condition Pushdown](/replication/optimization-and-tuning/query-optimizations/index-condition-pushdown/) — Index Condition Pushdown optimization
- [Query Limits and Timeouts](/replication/optimization-and-tuning/query-optimizations/query-limits-and-timeouts/) — Different methods MariaDB provides to limit/timeout a query.
- [Aborting Statements that Exceed a Certain Time to Execute](/replication/optimization-and-tuning/query-optimizations/aborting-statements/) — Aborting statements that take longer than a certain time to execute.
- [Partition Pruning and Selection](/mariadb-administration/partitioning-tables/partition-pruning-and-selection/) — Partition pruning is when the optimizer knows which partitions are relevant for the query
- [Big DELETEs](/replication/optimization-and-tuning/query-optimizations/big-deletes/) — How to DELETE lots of rows from a large table
- [Data Sampling: Techniques for Efficiently Finding a Random Row](/replication/optimization-and-tuning/query-optimizations/data-sampling-techniques-for-efficiently-finding-a-random-row/) — Fetching random rows from a table (beyond ORDER BY RAND())
- [Data Warehousing High Speed Ingestion](/replication/optimization-and-tuning/query-optimizations/data-warehousing-high-speed-ingestion/) — Ingesting lots of data and performance is bottlenecked in the INSERT area. What to do?
- [Data Warehousing Summary Tables](/replication/optimization-and-tuning/query-optimizations/data-warehousing-summary-tables/) — Creation and maintenance of summary tables
- [Data Warehousing Techniques](/replication/optimization-and-tuning/query-optimizations/data-warehousing-techniques/) — Improving performance for data-warehouse-like tables
- [Equality propagation optimization](/replication/optimization-and-tuning/query-optimizations/equality-propagation-optimization/) — Basic idea
Consider a query with a WHERE clause:
WHERE col1=col2 AND ...
t...
- [FORCE INDEX](/replication/optimization-and-tuning/query-optimizations/force-index/) — Similar to USE INDEX, but tells the optimizer to regard a table scan as very expensive.
- [Groupwise Max in MariaDB](/replication/optimization-and-tuning/query-optimizations/groupwise-max-in-mariadb/) — Finding the largest row for each group
- [GUID/UUID Performance](/replication/optimization-and-tuning/query-optimizations/guiduuid-performance/) — GUID/UUID performance (type 1 only)
- [IGNORE INDEX](/replication/optimization-and-tuning/query-optimizations/ignore-index/) — Tell the optimizer to not consider a particular index.
- [Optimizing for "Latest News"-style Queries](/replication/optimization-and-tuning/query-optimizations/optimizing-for-latest-news-style-queries/) — Optimizing the schema and code for "Latest News"-style queries
- [Pagination Optimization](/replication/optimization-and-tuning/query-optimizations/pagination-optimization/) — Pagination, not with OFFSET, LIMIT
- [Pivoting in MariaDB](/replication/optimization-and-tuning/query-optimizations/pivoting-in-mariadb/) — Pivoting data so a linear list of values with two keys becomes a spreadsheet-like array
- [Rollup Unique User Counts](/replication/optimization-and-tuning/query-optimizations/rollup-unique-user-counts/) — Technique for counting unique users
- [Rowid Filtering Optimization](/replication/optimization-and-tuning/query-optimizations/rowid-filtering-optimization/) — Rowid filtering is an optimization available from MariaDB 10.4.
- [USE INDEX](/replication/optimization-and-tuning/query-optimizations/use-index/) — Find rows in the table using only one of the named indexes.