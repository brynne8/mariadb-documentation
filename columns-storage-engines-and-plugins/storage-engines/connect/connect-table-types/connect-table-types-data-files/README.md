# CONNECT Table Types - Data Files

Most of the tables processed by CONNECT are just plain DOS or UNIX data files,
logically regarded as tables thanks to the description given when creating
the table. This description comes from the [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table)
statement. Depending on the application, these tables can already exist as data
files, used as is by CONNECT, or can have been physically made by CONNECT as
the result of a `CREATE TABLE ... SELECT ...` and/or INSERT statement(s).

The file <em>path/name</em> is given by the `FILE_NAME` option. If it is a
relative path/name, it will be relative to the database directory, the one
containing the table `.FRM` file.

Unless specified, the maturity of file table types is stable.

## Multiple File Tables

A <strong>multiple</strong> file table is one that is physically contained in several files
of the same type instead of just one. These files are processed sequentially
during the process of a query and the result is the same as if all the table
files were merged into one. This is great to process files coming from
different sources (such as cash register log files) or made at different time
periods (such as bank monthly reports) regarded as one table. Note that the
operations on such files are restricted to sequential Select and Update; and
that VEC multiple tables are not supported by CONNECT. The file list depends on
the setting of the <strong>multiple</strong> option of the `CREATE TABLE` statement for
that table.

Multiple tables are specified by the option MULTIPLE=<em>n</em>, which can take
three values:

<table><tbody><tr><td>0</td><td>Not a multiple table (the default). This can be used in an alter table statement.</td></tr>
<tr><td>1</td><td>The table is made from files located in the same directory. The FILE_NAME option is a pattern such as <code>'cash*.log'</code> that all the table file path/names verify.</td></tr>
<tr><td>2</td><td>The FILE_NAME gives the name of a file that contains the path/names of all the table files. This file can be made using a DIR table.</td></tr>
</tbody></table>

The `FILEID` special column, described
[here](/columns-storage-engines-and-plugins/storage-engines/connect/using-connect/using-connect-virtual-and-special-columns), allows filtering the file
list or doing some grouping on the files that make a multiple table.

<strong>Note:</strong> Multiple was not initially implemented for XML tables. This
restriction is removed since version 1.02.

## File Mapping

For file-based tables of reasonable size, processing time can be greatly
enhanced under Windows(TM) and some flavors of UNIX or Linux by using the
technique of “file mapping”, in which a file is processed as if it were
entirely in memory. Mapping is specified when creating the table by the use of
the `MAPPED=YES` option. This does not apply to tables not handled by system
I/O functions (<a undefined>XML</a> and
 [INI](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-ini-table-type)).

## Big File Tables

Because all files are handled by the standard input/output functions of the
operating system, their size is limited to 2GB, the maximum size handled by
standard functions. For some table types, CONNECT can deal with files that are
larger than 2GB, or prone to become larger than this limit. These are the [FIX](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-dos-and-fix-table-types),
[BIN](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-bin-table-type) and [VEC](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-vec-table-type) types. To tell
connect to use input/output functions dealing with big files, specify the
option `huge=1` or `huge=YES` for that table. Note however that CONNECT
cannot randomly access tables having more than 2G records.

## Compressed File Tables

CONNECT can make and process some tables whose data file is compressed. The
only supported compression format is the gzlib format. Zip and zlib formats are
supported differently. The table types that can be compressed are
[DOS](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-dos-and-fix-table-types),
[FIX](connect-dos-and-fix-table-typess),
[BIN](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-bin-table-type),
[CSV](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-csv-and-fmt-table-types) and
[FMT](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-csv-and-fmt-table-types). This can save some disk space
at the cost of a somewhat longer processing time.

Some restrictions apply to compressed tables:

- Compressed tables are not indexable.
- Update and partial delete are not supported.

Use the numeric compress option to specify a compressed table:

1 Not compressed
2 Compressed in gzlib format.
3 Made of compressed blocks of block_size records (enabling block indexing)

## Relational Formatted Tables

These are based on files whose records represent one table row. Only the column representation within each record can differ. The following relational formatted tables are supported:

- [DOS and FIX Table Types](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-dos-and-fix-table-types)
- [DBF Table Type](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-dbf-table-type)
- [BIN Table Type](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-bin-table-type)
- [VEC Table Type](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-vec-table-type)
- [CSV and FMT Table Types](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-csv-and-fmt-table-types)

## NoSQL Table Types

These are based on files that do not match the relational format but often represent hierarchical data. CONNECT can handle JSON, INI-CFG, XML and some HTML files..

The way it is done is different from what PostgreSQL does. In addition to including in a table some column values of a specific data format (JSON, XML) to be handled by specific functions, CONNECT can directly use JSON, XML or INI files that can be produced by other applications and this is the table definition that describes where and how the contained information must be retrieved.

This is also different from what MariaDB does with [dynamic columns](/sql-statements-structure/nosql/dynamic-columns), which is close to what MySQL and PostgreSQL do with the JSON column type.

The following NoSQL types are supported:

- [XML Table Type](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-xml-table-type)
- [JSON Table Type](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-json-table-type)
- [INI Table Type](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-ini-table-type)

