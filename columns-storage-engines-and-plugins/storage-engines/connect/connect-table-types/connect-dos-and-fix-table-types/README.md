# CONNECT DOS and FIX Table Types

## Overview

Tables of type DOS and FIX are based on text files (see [CONNECT Table Types - Data Files](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-table-types-data-files)). Within a record, column fields are positioned at a fixed offset from
the beginning of the record. Except sometimes for the last field, column fields
are also of fixed length. If the last field has varying length, the type of the
table is DOS. For instance, having the file <em>dept.dat</em> formatted like:

```sql
0318 KINGSTON       70012 SALES       Bank/Insurance
0021 ARMONK         87777 CHQ         Corporate headquarter
0319 HARRISON       40567 SALES       Federal Administration
2452 POUGHKEEPSIE   31416 DEVELOPMENT Research & development
```

You can define a table based on it with:

```sql
create table department (
  number char(4) not null,
  location char(15) not null flag=5,
  director char(5) not null flag=20,
  function char(12) not null flag=26,
  name char(22) not null flag=38)
engine=CONNECT table_type=DOS file_name='dept.dat';
```

Here the flag column option represents the offset of this column inside the
records. If the offset of a column is not specified, it defaults to the end of
the previous column and defaults to 0 for the first one.  The <em>`lrecl`</em>
parameter that represents the maximum size of a record is calculated by default
as the end of the rightmost column and can be unspecified except when some
trailing information exists after the rightmost column.

<strong>Note:</strong> A special case is files having an encoding such as UTF-8 (for
instance specifying `charset=UTF8`) in which some characters may be
represented with several bytes. Unlike the type size that MariaDB interprets as
a number of characters, the `lrecl` value is the record size in bytes and the
flag value represents the offset of the field in the record in bytes. If the
flag and/or the `lrecl` value are not specified, they will be calculated by
the number of characters in the fields multiplied by a value that is the
maximum size in bytes of a character for the corresponding charset. For UTF-8
this value is 3 which is often far too much as there are very few characters
requiring 3 bytes to be represented. When creating a new file, you are on the
safe side by only doubling the maximum number of characters of a field to
calculate the offset of the next field. Of course, for already existing files,
the offset must be specified according to what it is in it.

Although the field representation is always text in the table file, you can
freely choose the corresponding column type, characters, date, integer or
floating point according to its contents.

Sometimes, as in the <em>number</em> column of the above <em>department</em> table, you
have the choice of the type, numeric or characters. This will modify how the
column is internally handled <span>—</span> in characters `0021`
is different from `21` but not in numeric <span>—</span> as well
as how it is displayed.

If the last field has fixed length, the table should be referred as having the
type `FIX`. For instance, to create a table on the file <em>boys.txt</em>:

```sql
John      Boston      25/01/1986  02/06/2010
Henry     Boston      07/06/1987  01/04/2008
George    San Jose    10/08/1981  02/06/2010
Sam       Chicago     22/11/1979  10/10/2007
James     Dallas      13/05/1992  14/12/2009
Bill      Boston      11/09/1986  10/02/2008
```

You can for instance use the command:

```sql
create table boys (
  name char(12) not null,
  city char(12) not null,
  birth date not null date_format='DD/MM/YYYY',
  hired date not null date_format='DD/MM/YYYY' flag=36)
engine=CONNECT table_type=FIX file_name='boys.txt' lrecl=48;
```

Here some <em>flag</em> options were not specified because the fields have no
intermediate space between them except for the last column. The offsets are
calculated by default adding the field length to the <em>offset</em> of the
preceding field. However, for formatted date columns, the offset in the file
depends on the format and cannot be calculated by default. For fixed files,
the <em>lrecl</em> option is the physical length of the record including the line
ending character(s). It is calculated by adding to the end of the last field 2
bytes under Windows (CRLF) or 1 byte under UNIX. If the file is imported from
another operating system, the `ENDING` option will have to be specified with
the proper value.

For this table, the last offset and the record length must be specified anyway
because the date columns have field length coming from their format that is not
known by CONNECT. Do not forget to add the line ending length to the total
length of the fields.

This table is displayed as:

<table><tbody><tr><th>name</th><th>city</th><th>birth</th><th>hired</th></tr>
<tr><td>John</td><td>Boston</td><td>1986-01-25</td><td>2010-06-02</td></tr>
<tr><td>Henry</td><td>Boston</td><td>1987-06-07</td><td>2008-04-01</td></tr>
<tr><td>George</td><td>San Jose</td><td>1981-08-10</td><td>2010-06-02</td></tr>
<tr><td>Sam</td><td>Chicago</td><td>1979-11-22</td><td>2007-10-10</td></tr>
<tr><td>James</td><td>Dallas</td><td>1992-05-13</td><td>2009-12-14</td></tr>
<tr><td>Bill</td><td>Boston</td><td>1986-09-11</td><td>2008-02-10</td></tr>
</tbody></table>

Whenever possible, the fixed format should be preferred to the varying one
because it is much faster to deal with fixed tables than with variable tables.
Sure enough, instead of being read or written record by record, FIX tables are
processed by blocks of `BLOCK_SIZE` records, resulting in far less
input/output operations to execute. The block size defaults to 100 if not
specified in the Create Table statement.

<strong>Note 1:</strong> It is not mandatory to declare in the table all the fields existing
in the source file. However, if some fields are ignored, the <em>flag</em> option of
the following field and/or the <em>lrecl</em> option will have to be specified.

<strong>Note 2:</strong> Some files have an EOF marker (CTRL+Z 1A) that can prevent the
table to be recognized as fixed because the file length is not a multiple of
the fixed record size. To indicate this, use in the option list the create
option EOF. For instance, if after creating the FIX table <em>xtab</em> on the
file <em>foo.dat</em> that you know have fixed record size, you get, when you try to
use it, a message such as:

```sql
File foo.dat is not fixed length, len=302587 lrecl=141
```

After checking that the LRECL default or specified specification is correct,
you can indicate to ignore that extra EOF character by:

```sql
alter table xtab option_list='eof=1';
```

Of course, you can specify this option directly in the Create statement. All
this applies to some other table types, in particular to BIN tables.

<strong>Note 3:</strong> The width of the fields is the length specified in the column
declaration. For instance for a column declared as:

```sql
number int(3) not null,
```

The field width in the file is 3 characters. This is the value used to
calculate the offset of the next field if it is not specified. If this length
is not specified, it defaults to the MySQL default type length.

## Specifying the Field Format

Some files have specific format for their numeric fields. For instance, the
decimal point is absent and/or the field should be filled with leading zeros.
To deal with such files, as well in reading as in writing, the format can be
specified in the `CREATE TABLE` column definition. The syntax of the field
format specification is:

```sql
Field_format='[Z][N][d]'
```

The optional parts of the format are:

<table><tbody><tr><td><strong>Z</strong></td><td>The field has leading zeros</td></tr>
<tr><td><strong>N</strong></td><td>No decimal point exist in the file</td></tr>
<tr><td><strong><em>d</em></strong></td><td>The number of decimals, defaults to the column precision</td></tr>
</tbody></table>

## Example

Let us see how it works in the following example. We define a table based on
the file xfmt.txt having eight fields of 12 characters:

```sql
create table xfmt (
  col1 double(12,3) not null,
  col2 double(12,3) not null field_format='4',
  col3 double(12,2) not null field_format='N3',
  col4 double(12,3) not null field_format='Z',
  col5 double(12,3) not null field_format='Z3',
  col6 double(12,5) not null field_format='ZN5',
  col7 int(12) not null field_format='N3',
  col8 smallint(12) not null field_format='N3')
engine=CONNECT table_type=FIX file_name='xfmt.txt';

insert into xfmt values(4567.056,4567.056,4567.056,4567.056,-23456.8,
    3.14159,4567,4567);
select * from xfmt;
```

The first row is displayed as:

<table><tbody><tr><th>COL1</th><th>COL2</th><th>COL3</th><th>COL4</th><th>COL5</th><th>COL6</th><th>COL7</th><th>COL8</th></tr>
<tr><td>4567.056</td><td>4567.056</td><td>4567.06</td><td>4567.056</td><td>-23456.800</td><td>3.14159</td><td>4567</td><td>4567</td></tr>
</tbody></table>

The number of decimals displayed for all float columns is the column precision, the second argument of the column type option. Of course, integer columns have no decimals, although their formats specify some.

More interesting is the file layout. To see it let us define another table
based on the same file but whose columns are all characters:

```sql
create table cfmt (
  col1 char(12) not null,
  col2 char(12) not null,
  col3 char(12) not null,
  col4 char(12) not null,
  col5 char(12) not null,
  col6 char(12) not null,
  col7 char(12) not null,
  col8 char(12) not null)
engine=CONNECT table_type=FIX file_name='xfmt.txt';
select * from cfmt;
```

The (transposed) display of the select command shows the file text layout for
each field. Below a third column was added in this document to comment this
result.

<table><tbody><tr><th>Column</th><th>Row 1</th><th>Comment (all numeric fields are written right justified)</th></tr>
<tr><td><strong>COL1</strong></td><td><code>4567.056</code></td><td>No format, the value was entered as is.</td></tr>
<tr><td><strong>COL2</strong></td><td><code>4567.0560</code></td><td>The format ‘4’ forces to write 4 decimals.</td></tr>
<tr><td><strong>COL3</strong></td><td><code>4567060</code></td><td>N3 → No decimal point. The last 3 digits are decimals. However, the second decimal was rounded because of the column precision.</td></tr>
<tr><td><strong>COL4</strong></td><td><code>00004567.056</code></td><td>Z → Leading zeros, 3 decimals (the column precision)</td></tr>
<tr><td><strong>COL5</strong></td><td><code>-0023456.800</code></td><td>Z3 → (Minus sign) leading zeros, 3 decimals.</td></tr>
<tr><td><strong>COL6</strong></td><td><code>000000314159</code></td><td>ZN5 → Leading zeros, no decimal point, 5 decimals.</td></tr>
<tr><td><strong>COL7</strong></td><td><code>4567000</code></td><td>N3 → No decimal point. The last 3 digits are decimals.</td></tr>
<tr><td><strong>COL8</strong></td><td><code>4567000</code></td><td>Same. Any decimals would be ignored.</td></tr>
</tbody></table>

<strong>Note:</strong> For columns internally using double precision floating-point numbers,
MariaDB limits the decimal precision of any calculation to the column
precision. The declared column precision should be at least the number of
decimals of the format to avoid a loss of decimals as it happened for `col3`
of the above example.