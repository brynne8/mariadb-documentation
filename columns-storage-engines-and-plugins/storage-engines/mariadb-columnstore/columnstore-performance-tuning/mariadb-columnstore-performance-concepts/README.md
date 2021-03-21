# MariaDB ColumnStore Performance Concepts

# Introduction

The high level components of the ColumnStore architecture are:

- User Module (UM): The UM is responsible for parsing the SQL requests into an optimized set of primitive job steps executed by one or more PM servers. The UM is thus responsible for query optimization and orchestration of query execution by the PM servers. While multiple UM instances can be deployed in a multi server deployment, a single UM is responsible for each individual query. A database load balancer such as MariaDB MaxScale can be deployed to appropriately balance external requests against individual UM servers.
- Performance Module (PM): The PM executes granular job steps received from a UM in a multi-threaded manner. ColumnStore allows distribution of the work across many Performance Modules.  The UM is composed of the MariaDB mysqld process and ExeMgr process.
- Extent Maps: ColumnStore maintains metadata about each column in a shared distributed object known as the Extent Map The UM server references the Extent Map to help assist in generating the correct primitive job steps. The PM server references the Extent Map to identify the correct disk blocks to read.  Each column is made up of one or more files and each file can contain multiple extents. As much as possible the system attempts to allocate contiguous physical storage to improve read performance.
- Storage: ColumnStore can use either local storage or shared storage (e.g. SAN or EBS) to store data. Using shared storage allows for data processing to fail over to another node automatically in case of a PM server failing.

# Data loading

The system supports full MVCC ACID transactional logic via Insert, Update, and Delete statements. The MVCC architecture allows for concurrent query and DML / batch load. Although DML is supported, the system is optimized more for batch inserts and so larger data loads should be achieved through a batch load. The most flexible and optimal way to load data is via the cpimport tool. This tool optimizes the load path and can be run centrally or in parallel on each pm server.

If the data contains a time or (time correlated ascending value) column then significant performance gains will be achieved if the data is sorted by this field and also typically queried with a where clause on that column. This is because the system records a minimum and maximum value for each extent providing for a system maintained range partitioning scheme. This allows the system to completely eliminate scanning an extent map if the query includes a where clause for that field limiting the results to a subset of extent maps.

# Query execution

MariaDB ColumnStore has it's own query optimizer and execution engine distinct from the MariaDB server implementation. This allows for scaling out query execution to multiple PM servers and to optimize for handling data stored as columns rather than rows. As such the factors influencing query performance are very different:

A query is first parsed by the MariaDB server <em>mysqld</em> process and passed through to the ColumnStore storage engine. This passes the request onto the <em>ExeMgr</em> process which is responsible for optimizing and orchestrating execution of the query. The <em>ExeMgr</em> optimizer creates a series of batch primitive steps that are executed on the PM nodes by the <em>PrimProc</em> processes. Since multiple PM servers can be deployed this allows for scale out execution of the queries by multiple servers. As much as possible the optimizer attempts to push query execution down to the PM server however certain operations inherently must be executed centrally by the <em>ExeMgr</em> process, for example final result ordering. Filtering, joins, aggregates, and group by are in general pushed down and executed at the PM level. At the PM level batch primitive steps are performed at a granular level where individual threads operate on individual 1K-8K blocks within an extent. This enables a larger multi core server to be fully consumed and scale out within a single server. The current batch primitive steps available in the system include:

- <strong>Single Column Scan</strong> - Scan one or more Extents for a given column based on a single column predicate, including: =, &lt;&gt;, in (list), between, isnull, etc. See first scan section of [performance configuration](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-performance-tuning/mariadb-columnstore-performance-related-configuration-settings/) for additional details on tuning this.
- <strong>Additional Single Column Filters</strong> - Project additional column(s) for any rows found by a previous scan and apply additional single column predicates as needed. Access of blocks is based on row identifier, going directly to the block(s). See additional column read section of [performance configuration](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-performance-tuning/mariadb-columnstore-performance-related-configuration-settings/) for additional details on tuning this.
- <strong>Table Level Filters</strong> - Project additional columns as required for any table level filters such as: column1 &lt; column2, or more advanced functions and expressions. Access of blocks is again based on row identifier, going directly to the block(s).
- <strong>Project Join Columns for Joins</strong> - Project additional join column(s) as needed for any join operations. Access of blocks is again based on row identifier, going directly to the block(s).  See join tuning section of[performance configuration](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-performance-tuning/mariadb-columnstore-performance-related-configuration-settings/) for additional details on tuning this.
- <strong>Execute Multi-Join</strong> - Apply one or more hash join operation against projected join column(s) and use that value to probe a previously built hash map. Build out tuples as need to satisfy inner or outer join requirements. Note: Depending on the size of the previously built hash map, the actual join behavior may be executed either on the server running PM processes, or the server running UM processes. In either case, the Batch Primitive Step is functionally identical.  See multi table join section of [performance configuration](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-performance-tuning/mariadb-columnstore-performance-related-configuration-settings/) for additional details on tuning this.
- <strong>Cross-Table Level Filters</strong> - Project additional columns from the range of rows for the Primitive Step as needed for any cross-table level filters such as: table1.column1 &lt; table2.column2, or more advanced functions and expressions. Access of blocks is again based on row identifier, going directly to the block(s).  When a pre-requisite join operation takes place on the UM, then this operation will also take place on the UM, otherwise it will occur on the PM.
- <strong>Aggregation/Distinct Operation Part 1</strong> - Apply any local group by, distinct, or aggregation operation against the set of joined rows assigned to a given Batch Primitive. Part 1 of This process is handled by Performance Modules
- <strong>Aggregation/Distinct Operation Part 2</strong> Apply any final group by, distinct, or aggregation operation against the set of joined rows assigned to a given Batch Primitive. This processing is handled by the User Module. See memory manageent section of [performance configuration](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-performance-tuning/mariadb-columnstore-performance-related-configuration-settings/) for additional details on tuning this.

# ColumnStore query execution paradigms

The following items should be considered when thinking about query execution in ColumnStore vs a row based store such as InnoDB.

## Data scanning and filtering

ColumnStore is optimized for large scale aggregation / OLAP queries over large data sets. As such indexes typically used to optimize query access for row based systems do not make sense since selectivity is low for such queries. Instead ColumnStore gains performance by only scanning necessary columns, utilizing system maintained partitioning, and utilizing multiple threads and servers to scale query response time.

Since ColumnStore only reads the necessary columns to resolve a query, only include the necessary columns required. For example select * will be significantly slower than select col1, col2 from table.

Datatype size is important. If say you have a column that can only have values 0 through 100 then declare this as a tinyint as this will be represented with 1 byte rather than 4 bytes for int. This will reduce the I/O cost by 4 times.

For string types an important threshold is char(9) and varchar(8) or greater. Each column storage file uses a fixed number of bytes per value. This enables fast positional lookup of other columns to form the row. Currently the upper limit for columnar data storage is 8 bytes. So for strings longer than this the system maintains an additional 'dictionary' extent where the values are stored. The columnar extent file then stores a pointer into the dictionary. So it is more expensive to read and process a varchar(8) column than a char(8) column for example. So where possible you will get better performance if you can utilize shorter strings especially if you avoid the dictionary lookup. All TEXT/BLOB data types in 1.1 onward utilize a dictionary and do a multiple block 8KB lookup to retrieve that data if required, the longer the data the more blocks are retrieved and the greater a potential performance impact.

In a row based system adding redundant columns adds to the overall query cost but in a columnar system a cost is only occurred if the column is referenced. Therefore additional columns should be created to support different access paths. For instance store a leading portion of a field in one column to allow for faster lookups but additionally store the long form value as another column. Scans on a shorter code or leading portion column will be faster.

ColumnStore will distribute function application across PM nodes for greater performance but this requires a distributed implementation of the function in addition to the MariaDB server implementation. See [Distributed Functions](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-sql-structure-and-commands/columnstore-distributed-functions/) for the full list.

## Joins

Hash joins are utilized by ColumnStore to optimize for large scale joins and avoid the need for indexes and the overhead of nested loop processing. ColumnStore maintains table statistics so as to determine the optimal join order. This is implemented by first identifying the small table side (based on extent map data) and materializing the necessary rows from that table for the join. If the size of this is less than the configuration setting "PmMaxMemorySmallSide" then the join is pushed down to the PMs for distributed processing. Otherwise the larger side rows are pulled up to the UM for joining in the UM where only the where clause on that side is executed across PMs. If the join is too large for UM memory then disk based join can be enabled to allow the query to complete.

## Aggregations

Similarly to scalar functions ColumnStore distributes aggregate evaluation as much as possible. However some post processing is required to combine the final results in the UM. Enough memory must exist on both the PM and UM to handle queries where there are a very large number of values in the aggregate column(s).

Aggregation performance is also influenced by the number of distinct aggregate column values. Generally you'll see that for the same number of rows 100 distinct values will compute faster than 10000 distinct values. This is due to increased memory management as well as transfer overhead.

Select count(*) is internally optimized to be select count(COL-N) where COL-N is the column that uses the least number of bytes for storage. For example it would be pick a char(1) column over int column because char(1) uses 1 byte for storage and int uses 4 bytes. The implementation still honors ANSI semantics in that select count(*) will include nulls in the total count as opposed to an explicit select(COL-N) which excludes nulls in the count.

## Order by and limit

Order by and limit are currently implemented at the very end by the mariadb server process on the temporary result set table. This means that the unsorted results must be fully retrieved before either are applied.  The performance overhead of this is relatively minimal on small to medium results but for larger results it can be significant.

## Complex queries

Subqueries are executed in sequence thus the subquery intermediate results must be materialized in the UM and then the join logic applies with the outer query.

Window functions are executed at the UM level due to the need for ordering of the window results. The ColumnStore window function engines uses a dedicated faster sort process.

## Partitioning

Automated system partitioning of columns is provided by ColumnStore. As data is loaded into extent maps, the system will capture and maintain min/max values of column data in that extent map. New rows are appended to each extent map until full at which point a new extent map is created. For column values that are ordered or semi-ordered this allows for very effective data partitioning. By using the min and max values, entire extent maps can be eliminated and not read to filter data. This generally works particularly well for time dimension / series data or similar values that increase over time.