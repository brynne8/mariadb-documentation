# ColumnStore Information Schema Tables

MariaDB ColumnStore has four Information Schema tables that expose information about the table and column storage. These tables were added in version 1.0.5 of ColumnStore and were heavily modified for 1.0.6.

## COLUMNSTORE_TABLES

The first table is the INFORMATION_SCHMEA.COLUMNSTORE_TABLES. This contains information about the tables inside ColumnStore. The table layout is as follows:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td>TABLE_SCHEMA</td><td>The database schema for the table</td></tr>
<tr><td>TABLE_NAME</td><td>The table name</td></tr>
<tr><td>OBJECT_ID</td><td>The ColumnStore object ID for the table</td></tr>
<tr><td>CREATION_DATE</td><td>The date the table was created</td></tr>
<tr><td>COLUMN_COUNT</td><td>The number of columns in the table</td></tr>
<tr><td>AUTOINCREMENT</td><td>The start autoincrement value for the table set during CREATE TABLE</td></tr>
</tbody></table>

<strong>Note:</strong> Tables created with ColumnStore 1.0.4 or lower will have the year field of the creation data set incorrectly by 1900 years.

## COLUMNSTORE_COLUMNS

The INFORMATION_SCHEMA.COLUMNSTORE_COLUMNS table contains information about every single column inside ColumnStore. The table layout is as follows:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td>TABLE_SCHEMA</td><td>The database schema for the table</td></tr>
<tr><td>TABLE_NAME</td><td>The table name for the column</td></tr>
<tr><td>COLUMN_NAME</td><td>The column name</td></tr>
<tr><td>OBJECT_ID</td><td>The object ID for the column</td></tr>
<tr><td>DICTIONARY_OBJECT_ID</td><td>The dictionary object ID for the column (NULL if there is no dictionary object</td></tr>
<tr><td>LIST_OBJECT_ID</td><td>Placeholder for future information</td></tr>
<tr><td>TREE_OBJECT_ID</td><td>Placeholder for future information</td></tr>
<tr><td>DATA_TYPE</td><td>The data type for the column</td></tr>
<tr><td>COLUMN_LENGTH</td><td>The data length for the column</td></tr>
<tr><td>COLUMN_POSITION</td><td>The position of the column in the table, starting at 0</td></tr>
<tr><td>COLUMN_DEFAULT</td><td>The default value for the column</td></tr>
<tr><td>IS_NULLABLE</td><td>Whether or not the column can be set to NULL</td></tr>
<tr><td>NUMERIC_PRECISION</td><td>The numeric precision for the column</td></tr>
<tr><td>NUMERIC_SCALE</td><td>The numeric scale for the column</td></tr>
<tr><td>IS_AUTOINCREMENT</td><td>Set to 1 if the column is an autoincrement column</td></tr>
<tr><td>COMPRESSION_TYPE</td><td>The type of compression (either "None" or "Snappy")</td></tr>
</tbody></table>

## COLUMNSTORE_EXTENTS

This table displays the extent map in a user consumable form. An extent is a collection of details about a section of data related to a columnstore column. A majority of columns in ColumnStore will have multiple extents and the columns table above can be joined to this one to filter results by table or column. The table layout is as follows:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td>OBJECT_ID</td><td>The object ID for the extent</td></tr>
<tr><td>OBJECT_TYPE</td><td>Whether this is a "Column" or "Dictionary" extent</td></tr>
<tr><td>LOGICAL_BLOCK_START</td><td>ColumnStore's internal start LBID for this extent</td></tr>
<tr><td>LOGICAL_BLOCK_END</td><td>ColumnStore's internal end LBID for this extent</td></tr>
<tr><td>MIN_VALUE</td><td>This minimum value stored in this extent</td></tr>
<tr><td>MAX_VALUE</td><td>The maximum value stored in this extent</td></tr>
<tr><td>WIDTH</td><td>The data width for the extent</td></tr>
<tr><td>DBROOT</td><td>The DBRoot number for the extent</td></tr>
<tr><td>PARTITION_ID</td><td>The parition ID for the extent</td></tr>
<tr><td>SEGMENT_ID</td><td>The segment ID for the extent</td></tr>
<tr><td>BLOCK_OFFSET</td><td>The block offset for the data file, each data file can contain multiple extents for a column</td></tr>
<tr><td>MAX_BLOCKS</td><td>The maximum number of blocks for the extent</td></tr>
<tr><td>HIGH_WATER_MARK</td><td>The last block committed to the extent (starting at 0)</td></tr>
<tr><td>STATE</td><td>The state of the extent (see below)</td></tr>
<tr><td>STATUS</td><td>The availability status for the column which is either "Available", "Unavailable" or "Out of service"</td></tr>
<tr><td>DATA_SIZE</td><td>The uncompressed data size for the extent calculated as (HWM + 1) * BLOCK_SIZE</td></tr>
</tbody></table>

<strong>Notes:</strong>

1 The state is "Valid" for a normal state, "Invalid" if a cpimport has completed but the table has not yet been accessed (min/max values will be invalid) or "Updating" if there is a DML statement writing to the column
2 In ColumnStore the block size is 8192 bytes
3 By default ColumnStore will write create an extent file of 256*1024*WIDTH bytes for the first partition, if this is too small then for uncompressed data it will create a file of the maximum size for the extent (MAX_BLOCKS * BLOCK_SIZE). Snappy always compression adds a header block.
4 Object IDs of less than 3000 are for internal tables and will not appear in any of the information schema tables
5 Prior to 1.0.12 / 1.1.2 DATA_SIZE was incorrectly calculated
6 HWM is set to zero for the lower segments when there are multiple segments in an extent file, these can be observed when BLOCK_OFFSET &gt; 0
7 When HWM is 0 the DATA_SIZE will show 0 instead of 8192 to avoid confusion when there is multiple segments in an extent file

## COLUMNSTORE_FILES

The columnstore_files table provides information about each file associated with extensions. Each extension can reuse a file at different block offsets so this is not a 1:1 relationship to the columnstore_extents table.

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td>OBJECT_ID</td><td>The object ID for the extent</td></tr>
<tr><td>SEGMENT_ID</td><td>The segment ID for the extent</td></tr>
<tr><td>PARTITION_ID</td><td>The partition ID for the extent</td></tr>
<tr><td>FILENAME</td><td>The full path and filename for the extent file, multiple extents for the same column can point to this file with different BLOCK_OFFSETs</td></tr>
<tr><td>FILE_SIZE</td><td>The disk file size for the extent</td></tr>
<tr><td>COMPRESSED_DATA_SIZE</td><td>The amount of the compressed file used, NULL if this is an uncompressed file</td></tr>
</tbody></table>

## Stored Procedures

A few stored procedures were added in 1.0.6 to provide summaries based on the information schema tables. These can be accessed from the COLUMNSTORE_INFO schema.

### total_usage()

The total_usage() procedure gives a total disk usage summary for all the columns in ColumnStore with the exception of the columns used for internal maintenance. It is executed using the following query:

```sql
> call columnstore_info.total_usage();
```

### table_usage()

The table_usage() procedure gives a the total data disk usage, dictionary disk usage and grand total disk usage per-table. It can be called in several ways, the first gives a total for each table:

```sql
> call columnstore_info.table_usage(NULL, NULL);
```

Or for a specific table, my_table in my_schema in this example:

```sql
> call columnstore_info.table_usage('my_schema', 'my_table');
```

You can also request all tables for a specified schema:

```sql
> call columnstore_info.table_usage('my_schema', NULL);
```

<strong>Note:</strong> The quotes around the table name are required, an error will occur without them.

### compression_ratio()

The compression_ratio() procedure calculates the average compression ratio across all the compressed extents in ColumnStore. It is called using:

```sql
> call columnstore_info.compression_ratio();
```

<strong>Note:</strong> The compression ratio is incorrectly calculated before versions 1.0.12 / 1.1.2