# ColumnStore Storage Architecture

When you create a table on MariaDB ColumnStore, the system creates at least one file per column in the table.  So, for instance, a table created with three columns would have a minimum of three, separately addressable logical objects created on a SAN or on the local disk of a Performance Module.

ColumnStore writes the table schema locally to /usr/local/mariadb/columnstore/mysql/db with all the other non-ColumnStore tables.  The data you write to the ColumnStore table is stored across the Performance Modules in DB Roots, which are located in /usr/local/mariadb/columnstore/datax.

<img src="/kb/en/columnstore-storage-architecture/+image/datastorage-diagram" alt="datastorage-diagram" title="datastorage-diagram">

## Extents

Each column in the table is stored independently in a logical measure of 8,388,608 rows called an Extent.  Extents for 1 byte datatypes consume 8MB; 2 byte datatypes require 16MB; 4 byte datatypes 32MB; 8 bytes 64MB; and variable size datatypes 64MB.  Once an Extent becomes full, ColumnStore creates a new Extent.  String columns greater than 8 characters store indexes in the main column file and actual values in separate dictionary files.

Extents are physically stored as a collection of blocks.  Each block is 8KB.  Every database block is uniquely identified by its Logical Block Identifier, or LBID.

The physical file ColumnStore writes to disk is called a segment file.  Once segment files reach the maximum number of extents, ColumnStore automatically creates a new segment file.  You can set the maximum number of extents in a segment file using `ExtentsPreSegmentFile` in the `ColumnStore.xml` file.  It should be set to a multiple of the number of DB Roots.  The default value is 2.

Collectively, all of a column's segment files for one or more extents for a partition.  This is the horizontal partitioning in ColumnStore.  Partitions are stored in hierarchical structures organized by segments, (that is, folders).  ColumnStore meta-stores maps file structure and location to the DB schema as well as in the `FilesPerColumnPartition` in the `ColumnStore.xml` file.  The default value is 4.  Additionally, by default, ColumnStore compresses data.

### Extent Maps

ColumnStore uses a smart structure called an Extent Map to provide a logical range for partitioning and remove the need for indexing, manual table partitioning, materialized views, summary tables and other structures and objects that row-based databases must implement for query performance.

Extents are logical blocks of space that exist within a physical segment file, and is anywhere between 8 and 64 MB in size.  Each Extent supports the same number of rows, with smaller data types using less disk space.  The Extent Map catalogs Extents to their corresponding blocks (LBID's), along with minimum and maximum values for the column's data within the Extent.

The primary Performance Module has a master copy of the Extent Map.  On system startup, the file is read into memory, then physically copied to all other participating User and Performance modules for disaster recovery and failover.  Nodes keep the Extent Map in memory for quick access.  As Extents are modified, updates are broadcast to participating nodes.

### Extent Elimination

Using the Extent Map, ColumnStore can perform logical range partitioning and only retrieve the blocks needed to satisfy the query.  This is done through Extent Elimination, the process of eliminating Extents from the results that don't meet the given join and filter conditions of the query, which reduces the overall I/O operations.

<img src="/kb/en/columnstore-storage-architecture/+image/extent-elimination" alt="extent-elimination" title="extent-elimination">

In Extent Elimination, ColumnStore scans the columns in join and filter conditions.  It then extracts the logical horizontal partitioning information of each extent along with the minimum and maximum values for the column to further eliminate Extents.  To eliminate an Extent when a column scan involves a filter, that filter is compared to the minimum and maximum values stored in each extent for the column.  If the filter value is outside the Extents minimum and maximum value range, ColumnStore eliminates the Extent.

This behavior is automatic and well suited for series, ordered, patterned and time-based data, where the data is loaded frequently and often referenced by time.  Any column with clustered values is a good candidate for Extent Elimination.

## Compression with Real-time Decompression

In columnar storage, similar data is stored within each column file, which allows for excellent compressibility.  While the actual space savings depends on the randomness of the data and the number of distinct values that exists, many data-sets show compression rates saving between 65% and 95% space.

ColumnStore optimizes its compression strategy for read performance from disk.  It is tuned to accelerate the decompression rate, maximizing the performance benefits when reading from disk.  This allows systems that are I/O bound to improve performance on disk reads.

By default, compression is turned on in ColumnStore.  In addition, you can enable or disable it at the table-level or column-level, or control it at the session-level by setting the <a undefined>infinidb_compression_type</a> system variable.  When enabled, ColumnStore uses snappy compression.

## Version Buffer

MariaDB ColumnStore uses the Version Buffer to store disk blocks that are being modified, manage transaction rollbacks, and service the MVCC (multi-version concurrency control) or "snapshot read" function of the database.  This allows it to offer a query consistent view of the database.

All statements in ColumnStore run at a particular version (or, snapshot) of the database, which the system refers to as the System Change Number, (SCN).

<strong>Note:</strong> Although it is called a "buffer", the Version Buffer uses both memory and disk structures.

### How the Version Buffer Works

The Version Buffer uses in-memory hash tables to supply memory access to in-flight transaction information.  It starts at 4MB with the memory region growing from that amount to handle blocks that are being modified by a transaction.  Each entry in the hash table is a 40-byte reference to the 8KB block being modified.

The limiting factor of the Version Buffer is not the number of rows being updated, but rather the number of disk blocks.  You can increase the size, but use caution, since increasing the number of disk blocks means that [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update) and [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete) statements that run for long periods of time can take even longer in the event that you need to roll back the changes.

## Transaction Log

MariaDB ColumnStore supports logging committed transaction to the server's [Binary Log](/sql-statements-structure/sql-statements/administrative-sql-statements/binlog).