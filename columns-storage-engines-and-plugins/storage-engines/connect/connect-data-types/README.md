# CONNECT Data Types

Many data types make no or little sense when applied to plain files. This why
[CONNECT](/columns-storage-engines-and-plugins/storage-engines/connect/) supports only a restricted set of data types. However, ODBC, JDBC
or MYSQL source tables may contain data types not supported by CONNECT. In this
case, CONNECT makes an automatic conversion to a similar supported type when it
is possible.

The data types currently supported by CONNECT are:

<table><tbody><tr><th>Type name</th><th>Description</th><th>Used for</th></tr>
<tr><td><code>TYPE_STRING</code></td><td>Zero ended string</td><td><a href="/kb/en/char/">char</a>, <a href="/kb/en/varchar/">varchar</a>, <a href="/kb/en/text/">text</a></td></tr>
<tr><td><code>TYPE_INT</code></td><td>4 bytes integer</td><td><a href="/kb/en/int/">int</a>, <a href="/kb/en/mediumint/">mediumint</a>, <a href="/kb/en/integer/">integer</a></td></tr>
<tr><td><code>TYPE_SHORT</code></td><td>2 bytes integer</td><td><a href="/kb/en/smallint/">smallint</a></td></tr>
<tr><td><code>TYPE_TINY</code></td><td>1 byte integer</td><td><a href="/kb/en/tinyint/">tinyint</a></td></tr>
<tr><td><code>TYPE_BIGINT</code></td><td>8 bytes integer</td><td><a href="/kb/en/bigint/">bigint</a>, longlong</td></tr>
<tr><td><code>TYPE_DOUBLE</code></td><td>8 bytes floating point</td><td><a href="/kb/en/double/">double</a>, <a href="/kb/en/float/">float</a>, real</td></tr>
<tr><td><code>TYPE_DECIM</code></td><td>Numeric value</td><td><a href="/kb/en/decimal/">decimal</a>, numeric, number</td></tr>
<tr><td><code>TYPE_DATE</code></td><td>4 bytes integer</td><td><a href="/kb/en/date/">date</a>, <a href="/kb/en/datetime/">datetime</a>, <a href="/kb/en/time/">time</a>, <a href="/kb/en/timestamp/">timestamp</a>, <a href="/kb/en/year/">year</a></td></tr>
</tbody></table>

## TYPE_STRING

This type corresponds to what is generally known as [CHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/char/) or [VARCHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/varchar/) by
database users, or as strings by programmers. Columns containing characters
have a maximum length but the character string is of fixed or variable length
depending on the file format.

The DATA_CHARSET option must be used to specify the character set used in the
data source or file. Note that, unlike usually with MariaDB, when a multi-byte character set is used, the
column size represents the number of bytes the column value can contain, not
the number of characters.

## TYPE_INT

The ][INTEGER](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/integer/) type contains signed integer numeric 4-byte values (the <em>int/ of
the C language) ranging from `–2,147,483,648` to `2,147,483,647` for signed
type and `0` to `4,294,967,295` for unsigned type.</em>

## TYPE_SHORT

The SHORT data type contains signed [integer numeric 2-byte](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/smallint/) values (the <em>short
integer</em> of the C language) ranging from `–32,768` to `32,767` for signed
type and `0` to `65,535` for unsigned type.

## TYPE_TINY

The TINY data type contains [integer numeric 1-byte](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/tinyint/) values (the <em>char</em> of
the C language) ranging from `–128` to `127` for signed type and `0` to
`255` for unsigned type. For some table types, TYPE_TINY is used to represent Boolean values (0 is false, anything else is true).

## TYPE_BIGINT

The [BIGINT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/bigint/) data type contains signed integer 8-byte values (the <em>long long</em> of the C language) ranging from `-9,223,372,036,854,775,808` to
`9,223,372,036,854,775,807` for signed type and from `0` to
`18,446,744,073,709,551,615` for unsigned type.

Inside tables, the coding of all integer values depends on the table type. In
tables represented by text files, the number is written in characters, while in
tables represented by binary files (`BIN` or `VEC`) the number is directly
stored in the binary representation corresponding to the platform.

The <em>length</em> (or <em>precision</em>) specification corresponds to the length of
the table field in which the value is stored for text files only. It is used to
set the output field length for all table types.

## TYPE_DOUBLE

The DOUBLE data type corresponds to the C language [double](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/double/) type, a
floating-point double precision value coded with 8 bytes. Like for integers,
the internal coding in tables depends on the table type, characters for text
files, and platform binary representation for binary files.

The <em>length</em> specification corresponds to the length of the table field in
which the value is stored for text files only. The <em>scale</em> (was
 <em>precision</em>) is the number of decimal digits written into text files. For
binary table types (BIN and VEC) this does not apply. The <em>length</em> and
 <em>scale</em> specifications are used to set the output field length and number of
decimals for all types of tables.

## TYPE_DECIM

The DECIMAL data type corresponds to what MariaDB or ODBC data sources call NUMBER, NUMERIC, or [DECIMAL](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/decimal/): a numeric value with a maximum number of digits (the precision) some of them eventually being decimal digits (the scale). The internal coding in CONNECT is a character representation of the number. For instance:

```sql
colname decimal(14,6)
```

This defines a column <em>colname</em> as a number having a <em>precision</em> of 14 and
a <em>scale</em> of 6. Supposing it is populated by:

```sql
insert into xxx values (-2658.74);
```

The internal representation of it will be the character string
`-2658.740000`. The way it is stored in a file table depends on the table
type. The <em>length</em> field specification corresponds to the length of the table
field in which the value is stored and is calculated by CONNECT from the
 <em>precision</em> and the <em>scale</em> values. This length is <em>precision</em> plus 1 if
 <em>scale</em> is not 0 (for the decimal point) plus 1 if this column is not
unsigned (for the eventual minus sign). In fix formatted tables the number is
right justified in the field of width <em>length</em>, for variable formatted
tables, such as CSV, the field is the representing character string.

Because this type is mainly used by CONNECT to handle numeric or decimal fields
of ODBC, JDBC and MySQL table types, CONNECT does not provide decimal calculations or comparison by itself. This is why decimal columns of CONNECT tables cannot be indexed.

## DATE Data type

Internally, date/time values are stored by CONNECT as a signed 4-byte integer.
The value 0 corresponds to 01 January 1970 12:00:00 am coordinated universal
time ([UTC](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/coordinated-universal-time/)).  All other date/time values are
represented by the number of seconds elapsed since or before midnight
(00:00:00), 1 January 1970, to that date/time value. Date/time values before
midnight 1 January 1970 are represented by a negative number of seconds.

CONNECT handles dates from <strong>13 December 1901, 20:45:52</strong> to 
<strong>18 January 2038, 19:14:07</strong>.

Although date and time information can be represented in both CHAR and INTEGER
data types, the DATE data type has special associated properties. For each DATE
value, CONNECT can store all or only some of the following information:
century, year, month, day, hour, minute, and second.

### Date Format in Text Tables

Internally, date/time values are handled as a signed 4-byte integer. But in text
tables (type DOS, FIX, CSV, FMT, and DBF) dates are most of the time stored as
a formatted character string (although they also can be stored as a numeric
string representing their internal value). Because there are infinite ways to
format a date, the format to use for decoding dates, as well as the field
length in the file, must be associated to date columns (except when they are
stored as the internal numeric value).

Note that this associated format is used only to describe the way the temporal
value is stored internally. This format is used both for output to decode the
date in a SELECT statement as well as for input to encode the date in INSERT or
UPDATE statements. However, what is kept in this value depends on the data type
used in the column definition (all the MariaDB temporal values can be specified).
When creating a table, the format is associated to a date column using the
DATE_FORMAT option in the column definition, for instance:

```sql
create table birthday (
  Name varchar(17),
  Bday date field_length=10 date_format='MM/DD/YYYY',
  Btime time field_length=8 date_format='hh:mm tt')
engine=CONNECT table_type=CSV;

insert into birthday values ('Charlie','2012-11-12','15:30:00');

select * from birthday;
```

The SELECT query returns:

<table><tbody><tr><th>Name</th><th>Bday</th><th>Btime</th></tr>
<tr><td>Charlie</td><td>2012-11-12</td><td>15:30:00</td></tr>
</tbody></table>

The values of the INSERT statement must be specified using the standard MariaDB syntax and these values are displayed as MariaDB temporal values. Sure enough, the column formats apply only to the way these values are represented inside the CSV files. Here, the inserted record will be:

```sql
Charlie,11/12/2012,03:30 PM
```

<strong>Note:</strong> The field_length option exists because the MariaDB syntax does not allow specifying the field
length between parentheses for temporal column types. If not specified, the field length is calculated
from the date format (sometimes as a max value) or made equal to the default length value if there is no
date format. In the above example it could have been removed as the calculated values are the ones
specified. However, if the table type would have been DOS or FIX, these values could be adjusted to fit
the actual field length within the file.

A CONNECT format string consists of a series of elements that represent a
particular piece of information and define its format. The elements will be
recognized in the order they appear in the format string. Date and time format
elements will be replaced by the actual date and time as they appear in the
source string. They are defined by the following groups of characters:

<table><tbody><tr><th>Element</th><th>Description</th></tr>
<tr><td>YY</td><td>The last two digits of the year (that is, 1996 would be coded as "96").</td></tr>
<tr><td>YYYY</td><td>The full year (that is, 1996 could be entered as "96" but displayed as “1996”).</td></tr>
<tr><td>MM</td><td>The one or two-digit month number.</td></tr>
<tr><td>MMM</td><td>The three-character month abbreviation.</td></tr>
<tr><td>MMMM</td><td>The full month name.</td></tr>
<tr><td>DD</td><td>The one or two-digit month day.</td></tr>
<tr><td>DDD</td><td>The three-character weekday abbreviation.</td></tr>
<tr><td>DDDD</td><td>The full weekday name.</td></tr>
<tr><td>hh</td><td>The one or two-digit hour in 12-hour or 24-hour format.</td></tr>
<tr><td>mm</td><td>The one or two-digit minute.</td></tr>
<tr><td>ss</td><td>The one or two-digit second.</td></tr>
<tr><td>t</td><td>The one-letter AM/PM abbreviation (that is, AM is entered as "A").</td></tr>
<tr><td>tt</td><td>The two-letter AM/PM abbreviation (that is, AM is entered as "AM").</td></tr>
</tbody></table>

### Usage Notes

- To match the source string, you can add body text to the format string,
  enclosing it in single quotes or double quotes if it would be ambiguous.
  Punctuation marks do not need to be quoted.
- The hour information is regarded as 12-hour format if a “t” or “tt” element
  follows the “hh” element in the format or as 24-hour format otherwise.
- The "MM", "DD", "hh", "mm", "ss" elements can be specified with one or two
  letters (e.g. "MM" or "M") making no difference on input, but placing a
  leading zero to one-digit values on output<sup class="reference" id="_ref-0">[[1](#_note-0)]</sup> for two-letter elements.
- If the format contains elements DDD or DDDD, the day of week name is skipped
  on input and ignored to calculate the internal date value. On output, the
  correct day of week name is generated and displayed.
- Temporal values are always stored as numeric in [BIN](/kb/en/connect-table-types-data-files/#bin-table-type) and [VEC](/kb/en/connect-table-types-data-files/#vec-table-type-vecto) tables.

### Handling dates that are out of the range of supported CONNECT dates

If you want to make a table containing, for instance, historical dates not being convertible into CONNECT dates, make your column CHAR or VARCHAR and store the dates in the MariaDB format. All date functions applied to these strings will convert them to MariaDB dates and will work
as if they were real dates. Of course they must be inserted and will be displayed using the MariaDB format.

## NULL handling

CONNECT handles [null values](/kb/en/null-values-in-mariadb/) for data sources able to produce nulls. Currently
this concerns mainly the [ODBC](/kb/en/connect-table-types-odbc-table-type-accessing-tables-from-other-dbms/), [JDBC](/kb/en/connect-jdbc-table-type-accessing-tables-from-other-dbms/), MONGO, [MYSQL](/kb/en/connect-table-types-mysql-table-type-accessing-mysqlmariadb-tables/), [XML](/kb/en/connect-table-types-data-files/#xml-table-type), [JSON](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-json-table-type/) and [INI](/kb/en/connect-table-types-data-files/#ini-table-type) table types. For INI, [JSON](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-json-table-type/), MONGO or XML types, null values are returned when the key is missing in the section (INI) or when the corresponding node does not exist in a row (XML, JSON, MONGO).

For other file tables, the issue is to define what a null value is. In a numeric column, 0 can sometimes be a valid value but, in some other cases, it
can make no sense. The same for character columns; is a blank field a valid value or not?

A special case is DATE columns with a DATE_FORMAT specified. Any value not matching the format can be regarded as NULL.

CONNECT leaves the decision to you. When declaring a column in the [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/)
statement, if it is declared NOT NULL, blank or zero values will be considered
as valid values. Otherwise they will be considered as NULL values. In all
cases, nulls are replaced on insert or update by pseudo null values, a zero-length character string for text types or a zero value for numeric types. Once
converted to pseudo null values, they will be recognized as NULL only for
columns declared as nullable.

For instance:

```sql
create table t1 (a int, b char(10)) engine=connect;
insert into t1 values (0,'zero'),(1,'one'),(2,'two'),(null,'???');
select * from t1 where a is null;
```

The select query replies:

<table><tbody><tr><th>a</th><th>b</th></tr>
<tr><td>NULL</td><td>zero</td></tr>
<tr><td>NULL</td><td>???</td></tr>
</tbody></table>

Sure enough, the value 0 entered on the first row is regarded as NULL for a
nullable column. However, if we execute the query:

```sql
select * from t1 where a = 0;
```

This will return no line because a NULL is not equal to 0 in an SQL where clause.

Now let us see what happens with not null columns:

```sql
create table t1 (a int not null, b char(10) not null) engine=connect;
insert into t1 values (0,'zero'),(1,'one'),(2,'two'),(null,'???');
```

The insert statement will produce a warning saying:

<table><tbody><tr><th>Level</th><th>Code</th><th>Message</th></tr>
<tr><td>Warning</td><td>1048</td><td>Column 'a' cannot be null</td></tr>
</tbody></table>

It is replaced by a pseudo null `0` on the fourth row. Let us see the result:

```sql
select * from t1 where a is null;
select * from t1 where a = 0;
```

The first query returns no rows, 0 are valid values and not NULL. The second query replies:

<table><tbody><tr><th>a</th><th>b</th></tr>
<tr><td>0</td><td>zero</td></tr>
<tr><td>0</td><td>???</td></tr>
</tbody></table>

It shows that the NULL inserted value was replaced by a valid 0 value.

## Unsigned numeric types

They are supported by CONNECT since version 1.01.0010 for fixed numeric types
(TINY, SHORT, INTEGER, and BITINT).

## Data type conversion

CONNECT is able to convert data from one type to another in most cases. These
conversions are done without warning even when this leads to truncation or loss
of precision. This is true, in particular, for tables of type ODBC, JDBC, MYSQL and PROXY (via MySQL)
because the source table may contain some data types not supported by CONNECT.
They are converted when possible to CONNECT types.

When converted, MariaDB types are converted as:

<table><tbody><tr><th>MariaDB Types</th><th>CONNECT Type</th><th>Remark</th></tr>
<tr><td><a href="/kb/en/integer/">integer</a>, <a href="/kb/en/mediumint/">medium integer</a></td><td>TYPE_INT</td><td>4 byte integer</td></tr>
<tr><td><a href="/kb/en/smallint/">small integer</a></td><td>TYPE_SHORT</td><td>2 byte integer</td></tr>
<tr><td><a href="/kb/en/tinyint/">tiny integer</a></td><td>TYPE_TINY</td><td>1 byte integer</td></tr>
<tr><td><a href="/kb/en/char/">char</a>, <a href="/kb/en/varchar/">varchar</a></td><td>TYPE_STRING</td><td>Same length</td></tr>
<tr><td><a href="/kb/en/double/">double</a>, <a href="/kb/en/float/">float</a>, real</td><td>TYPE_DOUBLE</td><td>8 byte floating point</td></tr>
<tr><td><a href="/kb/en/decimal/">decimal</a>, numeric</td><td>TYPE_DECIM</td><td>Length depends on precision and scale</td></tr>
<tr><td>all <a href="/kb/en/date/">date</a> related types</td><td>TYPE_DATE</td><td>Date format can be set accordingly</td></tr>
<tr><td><a href="/kb/en/bigint/">bigint</a>, longlong</td><td>TYPE_BIGINT</td><td>8 byte integer</td></tr>
<tr><td><a href="/kb/en/enum/">enum</a>, <a href="/kb/en/set-data-type/">set</a></td><td>TYPE_STRING</td><td>Numeric value not accessible</td></tr>
<tr><td>All text types</td><td>TYPE_STRING <br>TYPE_ERROR</td><td>Depending on the value of the <a href="/kb/en/connect-system-variables/#connect_type_conv">connect_type_conv</a> system variable value.</td></tr>
<tr><td>Other types</td><td>TYPE_ERROR</td><td>Not supported, no conversion provided.</td></tr>
</tbody></table>

For [ENUM](/columns-storage-engines-and-plugins/data-types/string-data-types/enum/), the length of the column is the length of the longest value of the enumeration. For [SET](/columns-storage-engines-and-plugins/data-types/string-data-types/set-data-type/) the length is enough to contain all the set values concatenated with comma separator.

In the case of [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text/) columns, the handling depends on the values given to the [connect_type_conv](/kb/en/connect-system-variables/#connect_type_conv) and
[connect_conv_size](/kb/en/connect-system-variables/#connect_conv_size) system variables.

Note: [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob/) is currently not converted by default until a TYPE_BIN type is added to CONNECT. However, the FORCE option (from Connect 1.06.006) can be specified for blob columns containing text and the SKIP option also applies to ODBC BLOB columns.

ODBC SQL types are converted as:

<table><tbody><tr><th>SQL Types</th><th>Connect Type</th><th>Remark</th></tr>
<tr><td>SQL_CHAR, SQL_VARCHAR</td><td>TYPE_STRING</td><td></td></tr>
<tr><td>SQL_LONGVARCHAR</td><td>TYPE_STRING</td><td><code>len = min(abs(len), connect_conv_size)</code> If the column is generated by discovery (columns not specified) its length is <a href="/kb/en/connect-system-variables/#connect_conv_size">connect_conv_size</a>.</td></tr>
<tr><td>SQL_NUMERIC, SQL_DECIMAL</td><td>TYPE_DECIM</td><td></td></tr>
<tr><td>SQL_INTEGER</td><td>TYPE_INT</td><td></td></tr>
<tr><td>SQL_SMALLINT</td><td>TYPE_SHORT</td><td></td></tr>
<tr><td>SQL_TINYINT, SQL_BIT</td><td>TYPE_TINY</td><td></td></tr>
<tr><td>SQL_FLOAT, SQL_REAL, SQL_DOUBLE</td><td>TYPE_DOUBLE</td><td></td></tr>
<tr><td>SQL_DATETIME</td><td>TYPE_DATE</td><td><code>len = 10</code></td></tr>
<tr><td>SQL_INTERVAL</td><td>TYPE_STRING</td><td><code>len = 8 + ((scale) ? (scale+1) : 0)</code></td></tr>
<tr><td>SQL_TIMESTAMP</td><td>TYPE_DATE</td><td><code>len = 19 + ((scale) ? (scale +1) : 0)</code></td></tr>
<tr><td>SQL_BIGINT</td><td>TYPE_BIGINT</td><td></td></tr>
<tr><td>SQL_GUID</td><td>TYPE_STRING</td><td>l<code>len=36</code></td></tr>
<tr><td>SQL_BINARY, SQL_VARBINARY, SQL_LONG-VARBINARY</td><td>TYPE_STRING</td><td><code>len = min(abs(len), connect_conv_size</code>) Only if the value of <a href="/kb/en/connect-system-variables/#connect_type_conv">connect_type_conv</a> is <code>force</code>. The column should use the binary charset.</td></tr>
<tr><td>Other types</td><td>TYPE_ERROR</td><td><em>Not supported.</em></td></tr>
</tbody></table>

JDBC SQL types are converted as:

<table><tbody><tr><th>JDBC Types</th><th>Connect Type</th><th>Remark</th></tr>
<tr><td>(N)CHAR, (N)VARCHAR</td><td>TYPE_STRING</td></tr>
<tr><td>LONG(N)VARCHAR</td><td>TYPE_STRING</td><td><code>len = min(abs(len), connect_conv_size)</code> If the column is generated by discovery (columns not specified), its length is <a href="/kb/en/connect-system-variables/#connect_conv_size">connect_conv_size</a></td></tr>
<tr><td>NUMERIC, DECIMAL, VARBINARY</td><td>TYPE_DECIM</td></tr>
<tr><td>INTEGER</td><td>TYPE_INT</td></tr>
<tr><td>SMALLINT</td><td>TYPE_SHORT</td></tr>
<tr><td>TINYINT, BIT</td><td>TYPE_TINY</td></tr>
<tr><td>FLOAT, REAL, DOUBLE</td><td>TYPE_DOUBLE</td></tr>
<tr><td>DATE</td><td>TYPE_DATE</td><td><code>len = 10</code></td></tr>
<tr><td>TIME</td><td>TYPE_DATE</td><td><code>len = 8 + ((scale) ? (scale+1) : 0)</code></td></tr>
<tr><td>TIMESTAMP</td><td>TYPE_DATE</td><td><code>len = 19 + ((scale) ? (scale +1) : 0)</code></td></tr>
<tr><td>BIGINT</td><td>TYPE_BIGINT</td></tr>
<tr><td>Other types</td><td>TYPE_ERROR</td><td>Not supported.</td></tr>
</tbody></table>

Note: The [connect_type_conv](/kb/en/connect-system-variables/#connect_type_conv) SKIP option also applies to ODBC and JDBC tables.

---

1 [↑](#_ref-0) Here input and output are
  used to specify respectively decoding the date to get its numeric value from
  the data file and encoding a date to write it in the table file. Input is
  performed within [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/) queries; output is performed in [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update/) or [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert/)
  queries.