# MariaDB ColumnStore Performance Related Configuration Settings

# Introduction

A number of system configuration variables exist to allow fine tuning of the system to suit the physical hardware and query characteristics. In general the default values will work relatively well for many cases.

The configuration parameters are maintained in the /usr/local/mariadb/columnstore/etc/Columnstore.xml file. In a multiple server deployment these should only be edited on the PM1 server as this will be automatically replicated to other servers by the system.  A system restart will be required for the configuration change to take affect.

Convenience utility programs <em>getConfig</em> and <em>setConfig</em> are available to safely update the Columnstore.xml without needing to be comfortable with editing XML files. The -h argument will display usage information.

# Memory management - <em>NumBlocksPct</em> and <em>TotalUmMemory</em>

The <em>NumBlocksPct</em> configuration parameter specifies the percentage of physical memory to utilize for disk block caching. For a Single Server or Combined Multi Server deployment the default value is 50 to ensure enough physical memory for the UM and for a non combined multi serve deployment the default value is 70.

The <em>TotalUmMemory</em> configuration parameter specifies the percentage of physical memory to utilize for joins, intermediate results and set operations on the UM. This specifies an upper limit for small table results in joins rather than a pre-allocation of memory.

In a single server or combined deployment, the sum of <em>NumBlocksPct</em> and <em>TotalUmMemory</em> should typically not exceed 75% of physical memory. With very large memory servers this could be raised but the key point is to leave enough memory for other processes including mysqld.

With version 1.2.2 onwards these can be set to static numeric limits instead of percentages by entering a number with 'M' or 'G' at the end to signify MiB or GiB.

# Query concurrency - <em>MaxOutstandingRequests</em>

ColumnStore handles concurrent query execution by managing the rate of concurrent batch primitive steps from the UM to the PM. This is configured using the <em>MaxOutstandingRequests</em> parameter and has a default value of 20. Each batch primitive step is executed within the context of 1 extent column according to this high level process:

- The UM issues up to <em>MaxOutstandingRequests</em> number of batch primitive steps.
- The PM processes the request using many threads and returns its response. These generally take a fraction of a second up to a low number of seconds depending on the amount of Physical I/O and the performance of that storage.
- The UM will issue new requests as prior requests complete maintaining the maximum number of outstanding requests.

This scheme allows for large queries to use all available resources when not otherwise being consumed and for smaller queries to execute with minimal delay. Lower values optimize for higher throughput of smaller queries while a larger value optimizes for response time of that query. The default value should work well under most circumstances however the value should be increased as the number of PM nodes is increased.

How many Queries are running and how many queries are currently in the queue can be checked with

```sql
 select calgetsqlcount();
```

# Join tuning - <em>PmMaxMemorySmallSide</em>

ColumnStore maintains  statistics for table and utilizes this to determine which is the larger table of the two. This is based both on the number of blocks in that table and estimation of the predicate cardinality. The first step is to apply any filters as appropriate to the smaller table and returning this data set to the UM. The size of this data set is compared against the configuration parameter <em>PmMaxMemorySmallSide</em> which has a default value of 64 (MB). This value can be set all the way up to 4GB.  This default allows for approximately 1M rows on the small table side to be joined against billions (or trillions) on the large table side. If the size of the small data set is less than <em>PmMaxMemorySmallSide</em> the dataset will be sent to the PM for creation of a distributed hashmap otherwise it is created on the UM. Thus this setting is important to tuning of joins and whether the operation can be distributed or not. This should be set to support your largest expected small table join size up to available memory:

- Although this will increase the size of data sent from the UM to PM to support the join, it means that the join and subsequent aggregates are pushed down, scaled out, and a smaller data set is returned back to the UM.
- In a multiple PM deployment, the sizing should be based from available physical memory on the PM servers, how much memory to reserve for block caching, and the number of simultaneous join operations that can be expected to run times the average small table join data size.

# Multi table join tuning

The above logic for a single table join extrapolates out to multi table joins where the small table values are precalculated and performed as one single scan against the large table. This works well for the typical star schema case joining multiple dimension tables with a large fact table. For some join scenarios it may be necessary to sequence joins to create the intermediate datasets for joining, this would happen for instance with a snowflake schema structure. In some extreme cases it may be hard for the optimizer to be able to determine the most optimal join path. In this case a hint is available to force a join ordering. The INFINIDB_ORDERED hint will force the first table in the from clause to be considered the largest table and override any statistics based decision, for example:

```sql
select /*! INFINIDB_ORDERED */ r_regionkey     
from region r, customer c, nation n    
where r.r_regionkey = n.n_regionkey      
and n.n_nationkey = c.c_nationkey
```

Note:
INFINIDB_ORDERED is deprecated and not work anymore for 1.2 and above.

use
set infinidb_ordered_only=ON;

and for 1.4
set columnstore_ordered_only=ON;

# Disk based joins - <em>AllowDiskBasedJoin</em>

When a join is very large and exceeds the <em>PmMaxMemorySmallSide</em> setting it is performed in memory in the UM server. For very large joins this could exceed the available memory in which case this is detected and a query error reported. A number of configuration parameters are available to enable and configure usage of disk overflow should this occur:

- <em>AllowDiskBasedJoin</em> – Controls the option to use disk Based joins or not.  Valid values are Y (enabled) or N (disabled).  By default, this option is disabled.
- <em>TempFileCompression</em> – Controls whether the disk join files are compressed or noncompressed.  Valid values are Y (use compressed files) or N (use non-compressed files).
- <em>TempFilePath</em> – The directory path used for the disk joins.  By default, this path is the tmp directory for your installation (i.e., /usr/local/mariadb/columnstore/tmp).  Files (named infinidb-join-data*) in this directory will be created and cleaned on an as needed basis.   The entire directory is removed and recreated by ExeMgr at startup. <strong>It is strongly recommended that this directory is stored on a dedicated partition</strong>.

A mariadb global or session variable is available to specify a memory limit at which point the query is switched over to disk based joins:

- infinidb_um_mem_limit - Memory limit in MB per user (i.e. switch to disk based join if this limit is exceeded).  By default, this limit is not set (value of 0).