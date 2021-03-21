# CONNECT BIN Table Type

## Overview

A table of type BIN is physically a binary file in which each row is a logical
record of fixed length<sup class="reference" id="_ref-0">[[1](#_note-0)]</sup>. Within a record, column fields are
of a fixed offset and length as with [FIX tables](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-dos-and-fix-table-types/). Specific to BIN tables
is that numerical values are internally encoded using native platform
representation, so no conversion is needed to handle numerical values in
expressions.

It is not required that the lines of a BIN file be separated by characters such
as CR and/or LF but this is possible. In such an event, the <em>lrecl</em> option must
be specified accordingly.

<strong>Note:</strong> Unlike for the [DOS and FIX types](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-dos-and-fix-table-types/), the width of the fields is the
length of their internal representation in the file. For instance for a column
declared as:

```sql
number int(5) not null,
```

The field width in the file is 4 characters, the size of a binary integer. This
is the value used to calculate the offset of the next field if it is not
specified. Therefore, if the next field is placed 5 characters after this one,
this declaration is not enough, and the flag option will have to be used on the
next field.

## Type Conversion in BIN Tables

Here are the correspondences between the column type and field format provided
by default:

<table><tbody><tr><th>Column type</th><th>File default format</th></tr>
<tr><td>Char(<em>n</em>)</td><td>Text of <em>n</em> characters.</td></tr>
<tr><td>Date</td><td>Integer (4 bytes)</td></tr>
<tr><td>Int(<em>n</em>)</td><td>Integer (4 bytes)</td></tr>
<tr><td>Smallint(<em>n</em>)</td><td>Short integer (2 bytes)</td></tr>
<tr><td>TinyInt(<em>n</em>)</td><td>Char (1 Byte)</td></tr>
<tr><td>Bigint(<em>n</em>)</td><td>Large integer (8 bytes)</td></tr>
<tr><td>Double(<em>n,d</em>)</td><td>Double floating point (8 bytes)</td></tr>
</tbody></table>

However, the column type need not necessarily match the field format within the
table file. In particular, this occurs for field formats that correspond to
numeric types that are not handled by CONNECT<sup class="reference" id="_ref-1">[[2](#_note-1)]</sup>. Indeed, BIN table files may
internally contain float numbers or binary numbers of any byte length in big-endian or little-endian representation<sup class="reference" id="_ref-2">[[3](#_note-2)]</sup>. Also, as in
[DOS or FIX types](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-dos-and-fix-table-types/) tables, you may want to handle some character fields as numeric or
vice versa.

This is why it is possible to specify the field format when it does not
correspond to the column type default using the <em>field_format</em> column option
in the [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/) statement. Here are the available field formats for BIN tables:

<table><tbody><tr><th>Field_format</th><th>Internal representation</th></tr>
<tr><td><strong>[n]{L or B or H}[n]</strong></td><td>n bytes binary number in little endian, big endian or host endian representation.</td></tr>
<tr><td><strong>C</strong></td><td>Characters string (<em>n</em> bytes)</td></tr>
<tr><td><strong>I</strong></td><td>integer (4 bytes)</td></tr>
<tr><td><strong>D</strong></td><td>Double float (8 bytes)</td></tr>
<tr><td><strong>S</strong></td><td>Short integer (2 bytes)</td></tr>
<tr><td><strong>T</strong></td><td>Tiny integer (1 byte)</td></tr>
<tr><td><strong>G</strong></td><td>Big integer (8 bytes)</td></tr>
<tr><td><strong>F</strong> or <strong>R</strong></td><td>Real or float (Floating point number on 4 bytes)</td></tr>
<tr><td><strong>X</strong></td><td>Use the default format field for the column type</td></tr>
</tbody></table>

All field formats (except the first one) are a one-character specification<sup class="reference" id="_ref-3">[[4](#_note-3)]</sup>.
'X' is equivalent to not specifying the field format. For the 'C' character
specification, <em>n</em> is the column width as specified with the column type. For one-column formats, the
number of bytes of the numeric fields corresponds to what it is on most
platforms. However, it could vary for some. The G, I, S and T formats are deprecated because they correspond to supported data types and may not be supported in future versions.

## Example

Here is an example of a BIN table. The file record layout is supposed to be:

```sql
NNNNCCCCCCCCCCIIIISSFFFFSS
```

Here N represents numeric characters, C any characters, I integer bytes,
S short integer bytes, and F float number bytes. The `IIII` field contains a
date in numeric format.

The table could be created by:

```sql
create table testbal (
fig int(4) not null field_format='C',
name char(10) not null,
birth date not null field_format='L',
id char(5) not null field_format='L2',
salary double(9,2) not null default 0.00 field_format='F',
dept int(4) not null field_format='L2')
engine=CONNECT table_type=BIN block_size=5 file_name='Testbal.dat';
```

Specifying the little-endian representation for binary values is not useful on most machines, but makes the create table statement portable on a machine using big endian, as well as the table file.

The field offsets and the file record length are calculated according the
column internal format and eventually modified by the field format. It is not
necessary to specify them for a packed binary file without line endings. If a line
ending is desired, specify the ending option or specify the `lrecl` option adding the ending width. The table
can be filled by:

```sql
insert into testbal values
  (5500,'ARCHIBALD','1980-01-25','3789',4380.50,318),
  (123,'OLIVER','1953-08-10','23456',3400.68,2158),
  (3123,'FOO','2002-07-23','888',default,318);
```

Note that the types of the inserted values must match the column type, not the
field format type.

The query:

```sql
select * from testbal;
```

returns:

<table><tbody><tr><th>fig</th><th>name</th><th>birth</th><th>id</th><th>salary</th><th>dept</th></tr>
<tr><td>5500</td><td>ARCHIBALD</td><td>1980-01-25</td><td>3789</td><td>4380.50</td><td>318</td></tr>
<tr><td>123</td><td>OLIVER</td><td>1953-08-10</td><td>23456</td><td>3400.68</td><td>2158</td></tr>
<tr><td>3123</td><td>FOO</td><td>2002-07-23</td><td>888</td><td>0.00</td><td>318</td></tr>
</tbody></table>

## Numeric fields alignment

In binary files, numeric fields and record length can be aligned on 4-or-8-byte boundaries to optimize performance on certain processors. This can be
modified in the OPTION_LIST with an "align" option ("packed" meaning `align=1` is the default).

---

1 [↑](#_ref-0) Sometimes it can be a physical record if LF or
CRLF have been written in the file.
2 [↑](#_ref-1) Most of these are obsolete because CONNECT supports all column types except float
3 [↑](#_ref-2) The default endian representation used in the table file can be specified by setting the ENDIAN option as ‘L’ or ‘B’ in the option list.
4 [↑](#_ref-3) It can be specified
with more than one character, but only the first one is significant.