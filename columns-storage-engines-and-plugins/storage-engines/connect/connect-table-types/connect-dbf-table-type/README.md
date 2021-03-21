# CONNECT DBF Table Type

## Overview

A table of type `DBF` is physically a dBASE III or IV formatted file (used by
many products like dBASE, Xbase, FoxPro etc.). This format is similar to the
[FIX](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-dos-and-fix-table-types/) type format with in addition a prefix giving the characteristics of the
file, describing in particular all the fields (columns) of the table.

Because `DBF` files have a header that contains Meta data about the file, in
particular the column description, it is possible to create a table based on an
existing `DBF` file without giving the column description, for instance:

```sql
create table cust engine=CONNECT table_type=DBF file_name='cust.dbf';
```

To see what CONNECT has done, you can use the `DESCRIBE`
or `SHOW CREATE TABLE` commands, and eventually modify some options with
the `ALTER TABLE` command.

The case of deleted lines is handled in a specific way for DBF tables. Deleted
lines are not removed from the file but are "soft deleted" meaning they are
marked as deleted. In particular, the number of lines contained in the file
header does not take care of soft deleted lines. This is why if you execute
these two commands applied to a DBF table named <em>tabdbf</em>:

```sql
select count(*) from tabdbf;
select count(*) from tabdbf where 1;
```

They can give a different result, the (fast) first one giving the number of
physical lines in the file and the second one giving the number of line that
are not (soft) deleted.

The commands UPDATE, INSERT, and DELETE can be used with DBF tables. The DELETE
command marks the deleted lines as suppressed but keeps them in the file. The
INSERT command, if it is used to populate a newly created table, constructs the
file header before inserting new lines.

<strong>Note:</strong> For DBF tables, column name length is limited to 11 characters and
field length to 256 bytes.

## Conversion of dBASE Data Types

CONNECT handles only types that are stored as characters.

<table><tbody><tr><th>Symbol</th><th>DBF Type</th><th>CONNECT Type</th><th>Description</th></tr>
<tr><td>B</td><td>Binary (string)</td><td>TYPE_STRING</td><td>10 digits representing a .DBT block number.</td></tr>
<tr><td>C</td><td>Character</td><td>TYPE_STRING</td><td>All OEM code page characters - padded with blanks to the width of the field.</td></tr>
<tr><td>D</td><td>Date</td><td>TYPE_DATE</td><td>8 bytes - date stored as a string in the format YYYYMMDD.</td></tr>
<tr><td>N</td><td>Numeric</td><td>TYPE_INT TYPE_BIGINT TYPE_DOUBLE</td><td>Number stored as a string, right justified, and padded with blanks to the width of the field.</td></tr>
<tr><td>L</td><td>Logical</td><td>TYPE_STRING</td><td>1 byte - initialized to 0x20 otherwise T or F.</td></tr>
<tr><td>M</td><td>Memo (string)</td><td>TYPE_STRING</td><td>10 digits representing a .DBT block number.</td></tr>
<tr><td>@</td><td>Timestamp</td><td>Not supported</td><td>8 bytes - two longs, first for date, second for time.  It is the number of days since  01/01/4713 BC.</td></tr>
<tr><td>I</td><td>Long</td><td>Not supported</td><td>4 bytes. Leftmost bit used to indicate sign, 0 negative.</td></tr>
<tr><td>+</td><td>Autoincrement</td><td>Not supported</td><td>Same as a Long</td></tr>
<tr><td>F</td><td>Float</td><td>TYPE_DOUBLE</td><td>Number stored as a string, right justified, and padded with blanks to the width of the field.</td></tr>
<tr><td>O</td><td>Double</td><td>Not supported</td><td>8 bytes - no conversions, stored as a double.</td></tr>
<tr><td>G</td><td>OLE</td><td>TYPE_STRING</td><td>10 digits representing a .DBT block number.</td></tr>
</tbody></table>

For the N numeric type, CONNECT converts it to TYPE_DOUBLE if the decimals value is not 0, to TYPE_BIGINT if the length value is greater than 10, else to TYPE_INT.

For M, B, and G types, CONNECT just returns the DBT number.

## Reading soft deleted lines of a DBF table

It is possible to read these lines by changing the read mode of the table. This
is specified by an option `READMODE` that can take the values:

<table><tbody><tr><td><strong>0</strong></td><td>Standard mode. This is the default option.</td></tr>
<tr><td><strong>1</strong></td><td>Read all lines including soft deleted ones.</td></tr>
<tr><td><strong>2</strong></td><td>Read only the soft deleted lines.</td></tr>
</tbody></table>

For example, to read all lines of the tabdbf table, you can do:

```sql
alter table tabdbf option_list='Readmode=1';
```

To come back to normal mode, specify READMODE=0.