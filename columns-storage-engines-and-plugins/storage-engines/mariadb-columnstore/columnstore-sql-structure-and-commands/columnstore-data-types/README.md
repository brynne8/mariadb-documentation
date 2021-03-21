# ColumnStore Data Types

ColumnStore supports the following data types:

## Numeric Data Types

<table><tbody><tr><th>Datatypes</th><th>Column Size</th><th>Description</th></tr>
<tr><td><a href="/kb/en/boolean/">BOOLEAN</a></td><td>1-byte</td><td>A synonym for "TINYINT(1)". Supported from version 1.2.0 onwards.</td></tr>
<tr><td><a href="/kb/en/tinyint/">TINYINT</a></td><td>1-byte</td><td>A very small integer. Numeric value with scale 0. Signed: -126 to +127. Unsigned: 0 to 253.</td></tr>
<tr><td><a href="/kb/en/smallint/">SMALLINT</a></td><td>2-bytes</td><td>A small integer. Signed: -32,766 to 32,767. Unsigned: 0 to 65,533.</td></tr>
<tr><td><a href="/kb/en/mediumint/">MEDIUMINT</a></td><td>3-bytes</td><td>A medium integer. Signed: -8388608 to 8388607. Unsigned: 0 to 16777215. Supported starting with MariaDB ColumnStore 1.4.2.</td></tr>
<tr><td><a href="/kb/en/int/">INTEGER/INT</a></td><td>4-bytes</td><td>A normal-size integer. Numeric value with scale 0.  Signed: -2,147,483,646 to 2,147,483,647. Unsigned: 0 to 4,294,967,293</td></tr>
<tr><td><a href="/kb/en/bigint/">BIGINT</a></td><td>8-bytes</td><td>A large integer. Numeric value with scale 0. Signed: -9,223,372,036,854,775,806 to+9,223,372,036,854,775,807 Unsigned: 0 to +18,446,744,073,709,551,613</td></tr>
<tr><td><a href="/kb/en/decimal/">DECIMAL/NUMERIC</a></td><td>2, 4, or 8 bytes</td><td>A packed fixed-point number that can have a specific total number of digits and with a set number of digits after a decimal. The maximum precision (total number of digits) that can be specified is 18.</td></tr>
<tr><td><a href="/kb/en/float/">FLOAT</a></td><td>4 bytes</td><td>Stored in 32-bit IEEE-754 floating point format. As such, the number of significant digits is about 6and the range of values is approximately +/- 1e38.The MySQL extension to specify precision and scale is not supported.</td></tr>
<tr><td><a href="/kb/en/double/">DOUBLE/REAL</a></td><td>8 bytes</td><td>Stored in 64-bit IEEE-754 floating point format. As such, the number of significant digits is about 15 and the range of values is approximately +/-1e308. The MySQL extension to specify precision and scale is not supported. “REAL” is a synonym for “DOUBLE”.</td></tr>
</tbody></table>

## String Data Types

<table><tbody><tr><th>Datatypes</th><th>Column Size</th><th>Description</th></tr>
<tr><td><a href="/kb/en/char/">CHAR</a></td><td>1, 2, 4, or 8 bytes</td><td>Holds letters and special characters of fixed length. Max length is 255. Default and minimum size is 1 byte.</td></tr>
<tr><td><a href="/kb/en/varchar/">VARCHAR</a></td><td>1, 2, 4, or 8 bytes or 8-byte token</td><td>Holds letters, numbers, and special characters of variable length. Max length = 8000 bytes or characters and minimum length = 1 byte or character.</td></tr>
<tr><td><a href="/kb/en/tinytext/">TINYTEXT</a></td><td>255 bytes</td><td>Holds a small amount of letters, numbers, and special characters of variable length. Supported from version 1.1.0 onwards.</td></tr>
<tr><td><a href="/kb/en/tinyblob/">TINYBLOB</a></td><td>255 bytes</td><td>Holds a small amount of binary data of variable length. Supported from version 1.1.0 onwards.</td></tr>
<tr><td><a href="/kb/en/text/">TEXT</a></td><td>64 KB</td><td>Holds letters, numbers, and special characters of variable length. Supported from version 1.1.0 onwards.</td></tr>
<tr><td><a href="/kb/en/blob/">BLOB</a></td><td>64 KB</td><td>Holds binary data of variable length. Supported from version 1.1.0 onwards.</td></tr>
<tr><td><a href="/kb/en/mediumtext/">MEDIUMTEXT</a></td><td>16 MB</td><td>Holds a medium amount of letters, numbers, and special characters of variable length. Supported from version 1.1.0 onwards.</td></tr>
<tr><td><a href="/kb/en/mediumblob/">MEDIUMBLOB</a></td><td>16 MB</td><td>Holds a medium amount of binary data of variable length. Supported from version 1.1.0 onwards.</td></tr>
<tr><td><a href="/kb/en/longtext/">LONGTEXT</a></td><td>1.96 GB</td><td>Holds a large amount of letters, numbers, and special characters of variable length. Supported from version 1.1.0 onwards.</td></tr>
<tr><td><a href="/kb/en/longblob/">LONGBLOB</a></td><td>1.96 GB</td><td>Holds a large amount of binary data of variable length. Supported from version 1.1.0 onwards.</td></tr>
</tbody></table>

## Date and Time Data Types

<table><tbody><tr><th>Datatypes</th><th>Column Size</th><th>Description</th></tr>
<tr><td><a href="/kb/en/date/">DATE</a></td><td>4-bytes</td><td>Date has year, month, and day. The internal representation of a date is a string of 4 bytes. The first 2 bytes represent the year, .5 bytes the month, and .75 bytes the day in the following format: YYYY-MM-DD. Supported range is 1000-01-01 to 9999-12-31.</td></tr>
<tr><td><a href="/kb/en/datetime/">DATETIME</a></td><td>8-bytes</td><td>A date and time combination. Supported range is 1000-01-01 00:00:00 to 9999-12-31 23:59:59. From version 1.2.0 microseconds are also supported.</td></tr>
<tr><td><a href="/kb/en/time/">TIME</a></td><td>8-bytes</td><td>Holds hour, minute, second and optionally microseconds for time. Supported range is '-838:59:59.999999' to '838:59:59.999999'. Supported from version 1.2.0 onwards.</td></tr>
<tr><td><a href="/kb/en/timestamp/">TIMESTAMP</a></td><td>4-bytes</td><td>Values are stored as the number of seconds since 1970-01-01 00:00:00 UTC, and optionally microseconds. The max value is currently 2038-01-19 03:14:07 UTC. Supported starting with MariaDB ColumnStore 1.4.2.</td></tr>
</tbody></table>

### Notes

- ColumnStore treats a zero-length string as a NULL value.
- As with core MariaDB, ColumnStore employs “saturation semantics” on integer values. This means that if a value is inserted into an integer field that is too big/small for it to hold (i.e. it is more negative or more positive than the values indicated above), ColumnStore will “saturate” that value to the min/max value indicated above as appropriate. For example, for a SMALLINT column, if 32800 is attempted, the actual value inserted will be 32767.
- ColumnStore largest negative and positive numbers appears to be 2 less than what MariaDB supports. ColumnStore reserves these for its internal use and they cannot be used. For example, if there is a need to store -128 in a column, the TINYINT datatype cannot be used; the SMALLINT datatype must be used instead. If the value -128 is inserted into a TINYINT column, ColumnStore will saturate it to -126 (and issue a warning).
- ColumnStore truncates rather than rounds decimal constants that have too many digits after the decimal point during bulk load and when running SELECT statements. For INSERT and UPDATE, however, the MariaDB parser will round such constants. You should verify that ETL tools used and any INSERT/UPDATEstatements only specify the correct number of decimal digits to avoid potential confusion.
- An optional display width may be added to the BIGINT, INTEGER/INT, SMALLINT &amp; TINYINT columns. As with core MariaDB tables, this value does not affect the internal storage requirements of the column nor does it affect the valid value ranges.
- All columns in ColumnStore are nullable and the default value for any column is NULL. You may optionally specify NOT NULL for any column and/or one with a DEFAULT value.
- Unlike other MariaDB storage engines, the actual storage limit for LONGBLOB/LONGTEXT is 2,100,000,000 bytes instead of 4GB per entry. MariaDB's client API is limited to a row length of 1GB.