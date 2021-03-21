# Using CONNECT - General Information

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

The CONNECT handler was introduced in [MariaDB 10.0](/kb/en/what-is-mariadb-100/).

The main characteristic of [CONNECT](/columns-storage-engines-and-plugins/storage-engines/connect/) is to enable accessing data scattered on a machine as if it was a centralized database. This, and the fact that that locking is not used by connect (data files are open and closed for each query) makes CONNECT very useful for importing or exporting data into or from a MariaDB database and also for all types of Business Intelligence applications. However, it is not suited for transactional applications.

For instance, the index type used by CONNECT is closer to bitmap indexing than to B-trees. It is very fast for retrieving result but not when updating is done. In fact, even if only one indexed value is modified in a big table, the index is entirely remade (yet this being four to five times faster than for a b-tree index). But normally in Business Intelligence applications, files are not modified so often.

If you are using CONNECT to analyze files that can be modified by an external process, the indexes are of course not modified by it and become outdated. Use the OPTIMIZE TABLE command to update them before using the tables based on them.

This means also that CONNECT is not designed to be used by centralized servers, which are mostly used for transactions and often must run a long time without human intervening.

### Performance

Performances vary a great deal depending on the table type. For instance, ODBC tables are only
retrieved as fast as the other DBMS can do. If you have a lot of queries to execute, the best way to optimize your work can be sometime to translate the data from one type to another. Fortunately this is very simple with CONNECT. Fixed formats like FIX, BIN or VEC tables can be created from slower ones by commands such as:

```sql
Create table fastable table_specs select * from slowtable;
```

`FIX` and `BIN` are often the better choice because the I/O functions are
done on blocks of `BLOCK_SIZE` rows. `VEC` tables can be very efficient for
tables having many columns only a few being used in each query. Furthermore,
for tables of reasonable size, the `MAPPED` option can very often speed up
many queries.

### Create Table statement

Be aware of the two broad kinds of CONNECT tables:

<table><tbody><tr><td><strong>Inward</strong></td><td>They are table whose file name is not specified at create. An empty file will be given a default name (<em>tabname.tabtype</em>) and will be populated like for other engines. They do not require the FILE privilege and can be used for testing purpose.</td></tr>
<tr><td><strong>Outward</strong></td><td>They are all other <code>CONNECT</code> tables and access external data sources or files. They are the true useful tables but require the <code>FILE</code> privilege.</td></tr>
</tbody></table>

### Drop Table statement

For outward tables, the [DROP TABLE](/sql-statements-structure/sql-statements/data-definition/drop/drop-table/) statement just removes the table definition but does not erase the table data. However, dropping an inward tables also erase the table data as well.

### Alter Table statement

Be careful using the [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/) statement. Currently the data compatibility is not tested and the modified definition can become incompatible with the data. In particular, Alter modifies the table definition only but does not modify the table data. Consequently, the table type should not be modified this way, except to correct an incorrect definition. Also adding, dropping or modifying columns may be wrong because the default offset values (when not explicitly given by the FLAG option) may be wrong when recompiled with missing columns.

Safe use of ALTER is for indexing, as we have seen earlier, and to change options such as MAPPED or HUGE those do not impact the data format but just the way the data file is accessed. Modifying the BLOCK_SIZE option is all right with FIX, BIN, DBF, split VEC tables; however it is unsafe for VEC tables that are not split (only one data file) because at their creation the estimate size has been made a multiple of the block size. This can cause errors if this estimate is not a multiple of the new value of the block size.

In all cases, it is safer to drop and re-create the table (outward tables) or to make another one from the table that must be modified.

### Update and Delete for File Tables

##### MariaDB starting with [10.0.15](/kb/en/mariadb-10015-release-notes/)

The [connect_use_tempfile](/kb/en/connect-system-variables/#connect_use_tempfile) variable was introduced in [MariaDB 10.0.15](/kb/en/mariadb-10015-release-notes/)

CONNECT can execute these commands using two different algorithms:

- It can do it in place, directly modifying rows (update) or moving rows (delete) within the table file. This is a fast way to do it in particular when indexing is used.
- It can do it using a temporary file to make the changes. This is required when updating variable record length tables and is more secure in all cases.

The choice between these algorithms depends on the session variable [connect_use_tempfile](/kb/en/connect-system-variables/#connect_use_tempfile).