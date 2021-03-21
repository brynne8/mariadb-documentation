# SQL Server and MariaDB Types Comparison

This page helps to map each SQL Server type to the matching MariaDB type.

## Numbers

In MariaDB, numeric types can be declared as `SIGNED` or `UNSIGNED`. By default, numeric columns are `SIGNED`, so not specifying either will not break compatibility with SQL Server.

When using `UNSIGNED` values, there is a potential problem with subtractions. When subtracting an `UNSIGNED` valued from another, the result is usually of an `UNSIGNED` type. But if the result is negative, this will cause an error. To solve this problem, we can enable the [NO_UNSIGNED_SUBTRACTION](/kb/en/sql-mode/#no_unsigned_subtraction) flag in sql_mode.

For more information see [Numeric Data Type Overview](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/numeric-data-type-overview).

### Integer Numbers

<table><tbody><tr><th>SQL Server Types</th><th>Size (bytes)</th><th>MariaDB Types</th><th>Size (bytes)</th><th>Notes</th></tr>
<tr><td><code>tinyint</code></td><td>1</td><td><a href="/kb/en/tinyint/">TINYINT</a></td><td>1</td></tr>
<tr><td><code>smallint</code></td><td>2</td><td><a href="/kb/en/smallint/">SMALLINT</a></td><td>2</td></tr>
<tr><td></td><td></td><td><a href="/kb/en/mediumint/">MEDIUMINT</a></td><td>3</td><td>Takes 3 bytes on disk, but 4 bytes in memory</td></tr>
<tr><td><code>int</code></td><td>1</td><td><a href="/kb/en/int/">INT</a> / <a href="/kb/en/integer/">INTEGER</a></td><td>4</td></tr>
<tr><td><code>bigint</code></td><td>8</td><td><a href="/kb/en/bigint/">BIGINT</a></td><td>8</td></tr>
</tbody></table>

### Real Numbers (approximated)

<table><tbody><tr><th>SQL Server Types</th><th>Precision</th><th>Size</th><th>MariaDB Types</th><th>Size</th></tr>
<tr><td><code>float(1-24)</code></td><td>7 digits</td><td>4</td><td><a href="/kb/en/float/">FLOAT(0-23)</a></td><td>4</td></tr>
<tr><td><code>float(25-53)</code></td><td>15 digist</td><td>8</td><td><a href="/kb/en/float/">FLOAT(24-53)</a></td><td>8</td></tr>
</tbody></table>

MariaDB supports an alternative syntax: `FLOAT(M, D)`. M is the total number of digits, and D is the number of digits after the decimal point.

See also: [Floating-point Accuracy](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/floating-point-accuracy).

#### Aliases

In SQL Server `real` is an alias for `float(24)`.

In MariaDB [DOUBLE](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/double), and [DOUBLE PRECISION](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/double-precision) are aliases for `FLOAT(24-53)`.

Normally, `REAL` is also a synonym for `FLOAT(24-53)`. However, the [sql_mode](/mariadb-administration/variables-and-modes/sql-mode) variable can be set with the `REAL_AS_FLOAT` flag to make `REAL` a synonym for `FLOAT(0-23)`.

### Real Numbers (Exact)

<table><tbody><tr><th>SQL Server Types</th><th>Precision</th><th>Size (bytes)</th><th>MariaDB Types</th><th>Precision</th><th>Size (bytes)</th></tr>
<tr><td><code>decimal</code></td><td>0 - 38</td><td>Up to 17</td><td><a href="/kb/en/decimal/">DECIMAL</a></td><td>0 - 38</td><td><a href="/kb/en/data-type-storage-requirements/#decimal">See table</a></td></tr>
</tbody></table>

MariaDB supports this syntax: `DECIMAL(M, D)`. M and D are both optional. M is the total number of digits (10 by default), and D is the number of digits after the decimal point (0 by default). In SQL Server, defaults are 18 and 0, respectively. The reason for this difference is that SQL standard imposes a default of 0 for D, but it leaves the implementation free to choose any default for M.

SQL Server `DECIMAL` is equivalent to MariaDB `DECIMAL(18)`.

#### Aliases

The following [aliases](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/dec-numeric-fixed) for `DECIMAL` are recognized in both SQL Server and MariaDB: `DEC`, `NUMERIC`. MariaDB also allows one to use `FIXED`.

### Money

SQL Server `money` and `smallmoney` types represent real numbers guaranteeing a very low level of approximation (five decimal digits are accurate), optionally associated with one of the supported currencies.

MariaDB doesn't have monetary types. To represent amounts of money:

- Store the currency in a separate column, if necessary. It's possible to use a foreign key to a currencies table, or the [ENUM](/columns-storage-engines-and-plugins/data-types/string-data-types/enum) type.
- Use a non-approximated type:
<ul start="1"><li>[DECIMAL](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/decimal) is very convenient, as it allows one to store the number as-is. But calculations are potentially slower.
</li><li>An integer type is faster for calculations. It is possible to store, for example, the amount of money multiplied by 100.
</li></ul>

There is a small incompatibility that users should be aware about. `money` and `smallmoney` are accurate to about 4 decimal digits. This means that, if you use enough decimal digits, operations on these types may produce different results than the results they would produce on MariaDB types.

### Bits

The [BIT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/bit) type is supported in MariaDB. Its maximum size is `BIT(64)`. The `BIT` type has a fixed length. If we insert a value which requires less bits than the ones that are allocated, zero-bits are padded on the left.

In MariaDB, binary values can be written in one of the following ways:

- `b'value'`
- `0value`
where `value` is a sequence of 0 and 1 digits. Hexadecimal syntax can also be used. For more details, see [Binary Literals](/sql-statements-structure/sql-language-structure/binary-literals) and [Hexadecimal Literals](/sql-statements-structure/sql-language-structure/hexadecimal-literals).

MariaDB and SQL Server have different sets of bitwise operators. See [Bit Functions and Operators](/built-in-functions/secondary-functions/bit-functions-and-operators).

## BOOLEAN Pseudo-Type

In SQL Server, it is common to use `bit` to represent boolean values. In MariaDB it is possible to do the same, but this is not a common practice.

A column can also be defined as [BOOLEAN](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/boolean) or `BOOL`, which is just a synonym for [TINYINT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/tinyint). `TRUE` and `FALSE` keywords also exist, but they are synonyms for 1 and 0. To understand what this implies, see [Boolean Literals](/sql-statements-structure/sql-language-structure/sql-language-structure-boolean-literals).

In MariaDB `'True'` and `'False'` are always strings.

## Date and Time

<table><tbody><tr><th>SQL Server Types</th><th>Range</th><th>Precision</th><th>Size (bytes)</th><th>MariaDB Types</th><th>Range</th><th>Size (bytes)</th><th>Precision</th><th>Notes</th></tr>
<tr><td><code>date</code></td><td>0001-01-01 - 9999-12-31</td><td>3</td><td>/</td><td><a href="/kb/en/date/">DATE</a></td><td>0001-01-01 - 9999-12-31</td><td>3</td><td>/</td><td>They cover the same range</td></tr>
<tr><td><code>datetime</code></td><td>1753-01-01 - 9999-12-31</td><td>8</td><td>0 to 3, rounded</td><td><a href="/kb/en/datetime/">DATETIME</a></td><td>001-01-01 - 9999-12-31</td><td>8</td><td>0 to 6</td><td>MariaDB values are not approximated, see below.</td></tr>
<tr><td><code>datetime2</code></td><td>001-01-01 - 9999-12-31</td><td>8</td><td>6 to 8</td><td><a href="/kb/en/datetime/">DATETIME</a></td><td>001-01-01 - 9999-12-31</td><td>8</td><td>0 to 6</td><td>MariaDB values are not approximated, see below.</td></tr>
<tr><td><code>smalldatetime</code></td><td></td><td></td><td></td><td><a href="/kb/en/datetime/">DATETIME</a></td><td></td><td></td><td></td></tr>
<tr><td><code>datetimeoffset</code></td><td></td><td></td><td></td><td><a href="/kb/en/datetime/">DATETIME</a></td><td></td><td></td><td></td></tr>
<tr><td><code>time</code></td><td></td><td></td><td></td><td><a href="/kb/en/time/">TIME</a></td><td></td><td></td><td></td></tr>
</tbody></table>

You may also consider the following MariaDB types:

- [TIMESTAMP](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/timestamp) has little to do with SQL Server's `timestamp`. In MariaDB it is the number of seconds elapsed since the beginning of 1970-01-01, with a decimal precision up to 6 digits (0 by default). The value can optionally be automatically set to the current timestamp on insert, on update, or both. It is not meant to be a unique row identifier.
- [YEAR](/built-in-functions/date-time-functions/year) is a 1-byte type representing years between 1901 and 2155, as well as 0000.

### Zero Values

MariaDB allows a special value where all the parts of a date are zeroes: `'0000-00-00'`. This can be disallowed by setting [sql_mode=NO_ZERO_DATE](/kb/en/sql-mode/#no_zero_date).

It is also possible to use values where only some date parts are zeroes, for example `'1994-01-00'` or `'1994-00-00'`. These values can be disallowed by setting [sql_mode=NO_ZERO_IN_DATE](/kb/en/sql-mode/#no_zero_in_date). They are not affected by `NO_ZERO_DATE`.

### Syntax

Several different date formats are understood. Typically used formats are `'YYYY-MM-DD'` and `YYYYMMDD`. Several separators are accepted.

The syntax defined in standard SQL and ODBC are understood - for example, `DATE '1994-01-01'` and `{d '1994-01-01'} `. Using these eliminates possible ambiguities in contexts where a temporal value could be interpreted as a string or as an integer.

See [Date and Time Literals](/sql-statements-structure/sql-language-structure/date-and-time-literals) for the details.

### Precision

For temporal types that include a day time, MariaDB allows a precision from 0 to 6 (microseconds), 0 being the default. The subsecond part is never approximated. It adds up to 3 bytes. See [Data Type Storage Requirements](/kb/en/data-type-storage-requirements/#microseconds) for the details.

## String and Binary

### Binary Strings

<table><tbody><tr><th>SQL Server Types</th><th>Size (bytes)</th><th>MariaDB Types</th><th>Notes</th></tr>
<tr><td><code>binary</code></td><td>1 to 8000</td><td><a href="/kb/en/varbinary/">VARBINARY</a> or <a href="/kb/en/blob-and-text-data-types/">BLOB</a></td><td>See below for <code>BLOB</code> types</td></tr>
<tr><td><code>varbinary</code></td><td>1 to 8000</td><td><a href="/kb/en/varbinary/">VARBINARY</a> or <a href="/kb/en/blob-and-text-data-types/">BLOB</a></td><td>See below for <code>BLOB</code> types</td></tr>
<tr><td><code>image</code></td><td>2^31-1</td><td><a href="/kb/en/varbinary/">VARBINARY</a> or <a href="/kb/en/blob-and-text-data-types/">BLOB</a></td><td>See below for <code>BLOB</code> types</td></tr>
</tbody></table>

The `VARBINARY` type is similar to `VARCHAR`, but stores binary byte strings, just like SQL Server `binary` does.

For large binary strings, MariaDB has four `BLOB` types, with different sizes. See [BLOB and TEXT Data Types](/columns-storage-engines-and-plugins/data-types/string-data-types/blob-and-text-data-types) for more information.

### Character Strings

One important difference between SQL Server and MariaDB is that <strong>in MariaDB character sets do not depend on types and collations</strong>. Character sets can be set at database, table or column level. If this is not done, the default character sets applies, which is specified by the [character_set_server](/kb/en/server-system-variables/#character_set_server) system variable.

To create a MariaDB table that is identical to a SQL Server table, <strong>it may be necessary to specify a character set for each string column</strong>. However, in many cases using UTF-8 will work.

<table><tbody><tr><th>SQL Server Types</th><th>Size (bytes)</th><th>MariaDB Types</th><th>Size (bytes)</th><th>Character set</th></tr>
<tr><td><code>char</code></td><td>1 to 8000</td><td><a href="/kb/en/char/">CHAR</a></td><td>0 to 255</td><td><code>utf8mb4</code> (1, 4)</td></tr>
<tr><td><code>varchar</code></td><td>1 to 8000</td><td><a href="/kb/en/varchar/">VARCHAR</a></td><td>0 to 65,532 (2)</td><td><code>utf8mb4</code> (1)</td></tr>
<tr><td><code>text</code></td><td>2^31-1</td><td><a href="/kb/en/blob-and-text-data-types/">TEXT</a></td><td>2^31-1</td><td><code>ucs2</code></td></tr>
<tr><td><code>nchar</code></td><td>2 to 8000</td><td><a href="/kb/en/char/">CHAR</a></td><td>0 to 255</td><td><code>utf16</code> or <code>ucs2</code> (3, 4)</td></tr>
<tr><td><code>nvarchar</code></td><td>2 to 8000</td><td><a href="/kb/en/varchar/">VARCHAR</a></td><td>0 to 65,532 (2) (5)</td><td><code>utf16</code> or <code>ucs2</code> (1) (3)</td></tr>
<tr><td><code>tnext</code></td><td>2^30 - 1</td><td><a href="/kb/en/blob-and-text-data-types/">TEXT</a></td><td>2^31-1</td><td><code>ucs2</code></td></tr>
</tbody></table>

<strong>Notes:</strong>

1) If SQL Server uses a non-unicode collation, a subset of UTF-8 is used. So it is possible to use a smaller character set on MariaDB too.

2) [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) has a maximum row length of 65,535 bytes. [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/blob-and-text-data-types) columns do not contribute to the row size, because they are stored separately (except for the first 12 bytes).

3) In SQL Server, UTF-16 is used if data contains Supplementary Characters, otherwise UCS-2 is used. If not sure, use `utf16` in MariaDB.

4) In SQL Server, the value of `ANSI_PADDING` determines if `char` values should be padded with spaces to their maximum length. In MariaDB, this depends on the [PAD_CHAR_TO_FULL_LENGTH](/kb/en/sql-mode/#pad_char_to_full_length) sql_mode flag.

5) See JSON, below.

## SQL Server Special Types

### rowversion

MariaDB does not have the `rowversion` type.

If the only purpose is to check if a row has been modified since its last read, a [TIMESTAMP](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/timestamp) column can be used instead. Its default value should be `ON UPDATE CURRENT_TIMESTAMP`. In this way, the timestamp will be updated whenever the column is modified.

A way to preserve much more information is to use a [temporal table](/kb/en/temporal-data-tables/). Past versions of the row will be preserved.

### sql_variant

MariaDB does not support the `sql_variant` type.

MariaDB is quite flexible about implicit and explicit [type conversions](/built-in-functions/string-functions/type-conversion). Therefore, for most cases storing the values as a string should be equivalent to using `sql_variant`.

Be aware that the maximum length of an `sql_variant` value is 8,000 bytes. In MariaDB, you may need to use `TINYBLOB`.

### uniqueidentifier

MariaDB does not support the `uniqueidentifier` type.

`uniqueidentifier` columns contain 16-bit GUIDs. MariaDB can generate unique values with the [UUID()](/built-in-functions/secondary-functions/miscellaneous-functions/uuid) or [UUID_SHORT()](/built-in-functions/secondary-functions/miscellaneous-functions/uuid_short) functions, and stored them in `BIT(128)` or `BIT(64)` columns, respectively.

### xml

MariaDB does not support the `xml` type.

XML data can be stored in string columns. MariaDB supports several XML functions.

## JSON

With SQL Server, typically JSON documents are stored in `nvarchar` columns in a text form.

MariaDB has a [JSON](/columns-storage-engines-and-plugins/data-types/string-data-types/json-data-type) pseudo-type that maps to [LONGTEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/longtext). However, from [MariaDB 10.5](/kb/en/what-is-mariadb-105/) the `JSON` pseudo-type also checks that the value is valid a JSON document.

MariaDB supports different JSON functions than SQL Server. MariaDB currently has more functions, and SQL Server syntax will not work. See [JSON functions](/built-in-functions/special-functions/json-functions) for more information.