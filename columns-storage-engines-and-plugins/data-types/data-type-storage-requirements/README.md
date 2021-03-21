# Data Type Storage Requirements

The following tables indicate the approximate data storage requirements for each data type.

## Numeric Data Types

<table><tbody><tr><th>Data Type</th><th>Storage Requirement</th></tr>
<tr><td><a href="/kb/en/tinyint/">TINYINT</a></td><td>1 byte</td></tr>
<tr><td><a href="/kb/en/smallint/">SMALLINT</a></td><td>2 bytes</td></tr>
<tr><td><a href="/kb/en/mediumint/">MEDIUMINT</a></td><td>3 bytes</td></tr>
<tr><td><a href="/kb/en/int/">INT</a></td><td>4 bytes</td></tr>
<tr><td><a href="/kb/en/bigint/">BIGINT</a></td><td>8 bytes</td></tr>
<tr><td><a href="/kb/en/float/">FLOAT</a>(p)</td><td>4 bytes if p &lt;= 24, otherwise 8 bytes</td></tr>
<tr><td><a href="/kb/en/double/">DOUBLE</a></td><td>8 bytes</td></tr>
<tr><td><a href="/kb/en/decimal/">DECIMAL</a></td><td>See table below</td></tr>
<tr><td><a href="/kb/en/bit/">BIT</a>(M)</td><td>(M+7)/8 bytes</td></tr>
</tbody></table>

Note that MEDIUMINT columns will require 4 bytes in memory (for example, in InnoDB buffer pool).

### Decimal

[Decimals](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/decimal) are stored using a binary format, with the integer and the fraction stored separately. Each nine-digit multiple requires 4 bytes, followed by a number of bytes for whatever remains, as follows:

<table><tbody><tr><th>Remaining digits</th><th>Storage Requirement</th></tr>
<tr><td>0</td><td>0 bytes</td></tr>
<tr><td>1</td><td>1 byte</td></tr>
<tr><td>2</td><td>1 byte</td></tr>
<tr><td>3</td><td>2 bytes</td></tr>
<tr><td>4</td><td>2 bytes</td></tr>
<tr><td>5</td><td>3 bytes</td></tr>
<tr><td>6</td><td>3 bytes</td></tr>
<tr><td>7</td><td>4 bytes</td></tr>
<tr><td>8</td><td>4 bytes</td></tr>
</tbody></table>

## String Data Types

In the descriptions below, `M` is the declared column length (in characters or in bytes), while `len` is the actual length in bytes of the value.

<table><tbody><tr><th>Data Type</th><th>Storage Requirement</th></tr>
<tr><td><a href="/kb/en/enum/">ENUM</a></td><td>1 byte for up to 255 enum values, 2 bytes for 256 to 65,535 enum values</td></tr>
<tr><td><a href="/kb/en/char/">CHAR(M)</a></td><td>M × w bytes, where w is the number of bytes required for the maximum-length character in the character set</td></tr>
<tr><td><a href="/kb/en/binary/">BINARY(M)</a></td><td>M bytes</td></tr>
<tr><td><a href="/kb/en/varchar/">VARCHAR(M)</a>, <a href="/kb/en/varbinary/">VARBINARY(M)</a></td><td>len + 1 bytes if column is 0 – 255 bytes, len + 2 bytes if column may require more than 255 bytes</td></tr>
<tr><td><a href="/kb/en/tinyblob/">TINYBLOB</a>, <a href="/kb/en/tinytext/">TINYTEXT</a></td><td>len + 1 bytes</td></tr>
<tr><td><a href="/kb/en/blob/">BLOB</a>, <a href="/kb/en/text/">TEXT</a></td><td>len + 2 bytes</td></tr>
<tr><td><a href="/kb/en/mediumblob/">MEDIUMBLOB</a>, <a href="/kb/en/mediumtext/">MEDIUMTEXT</a></td><td>len + 3 bytes</td></tr>
<tr><td><a href="/kb/en/longblob/">LONGBLOB</a>, <a href="/kb/en/longtext/">LONGTEXT</a></td><td>len + 4 bytes</td></tr>
<tr><td><a href="/kb/en/set/">SET</a></td><td>Given M members of the set, (M+7)/8 bytes, rounded up to 1, 2, 3, 4, or 8 bytes</td></tr>
</tbody></table>

In some [character sets](/kb/en/data-types-character-sets-and-collations/), not all characters use the same number of bytes. utf8 encodes characters with one to three bytes per character, while utf8mb4 requires one to four bytes per character.

When using field COMPRESSED attribute, 1 byte is reserved to metadata, for example VARCHAR(255) will use +2bytes instead of +1

### Examples

Assuming a single-byte character-set:

<table><tbody><tr><td>Value</td><td>CHAR(2)</td><td>Storage Required</td><td>VARCHAR(2)</td><td>Storage Required</td></tr>
<tr><td>''</td><td>'  '</td><td>2 bytes</td><td>''</td><td>1 byte</td></tr>
<tr><td>'1'</td><td>'1 '</td><td>2 bytes</td><td>'1'</td><td>2 bytes</td></tr>
<tr><td>'12'</td><td>'12'</td><td>2 bytes</td><td>'12'</td><td>3 bytes</td></tr>
</tbody></table>

## Date and Time Data Types

<table><tbody><tr><th>Data Type</th><th>Storage Requirement</th></tr>
<tr><td><a href="/kb/en/date/">DATE</a></td><td>3 bytes</td></tr>
<tr><td><a href="/kb/en/time/">TIME</a></td><td>3 bytes</td></tr>
<tr><td><a href="/kb/en/datetime/">DATETIME</a></td><td>8 bytes</td></tr>
<tr><td><a href="/kb/en/timestamp/">TIMESTAMP</a></td><td>4 bytes</td></tr>
<tr><td><a href="/kb/en/year-data-type/">YEAR</a></td><td>1 byte</td></tr>
</tbody></table>

### Microseconds

[MariaDB 5.3](/kb/en/what-is-mariadb-53/) and MySQL 5.6 introduced [microseconds](/built-in-functions/date-time-functions/microseconds-in-mariadb). The underlying storage implementations were different, but from [MariaDB 10.1](/kb/en/what-is-mariadb-101/), MariaDB defaults to the MySQL format (by means of the [mysql56_temporal_format](/kb/en/server-system-variables/#mysql56_temporal_format) variable). Microseconds have the following additional storage requirements:

#### MySQL 5.6+ and [MariaDB 10.1](/kb/en/what-is-mariadb-101/)+

<table><tbody><tr><th>Precision</th><th>Storage Requirement</th></tr>
<tr><td>0</td><td>0 bytes</td></tr>
<tr><td>1,2</td><td>1 byte</td></tr>
<tr><td>3,4</td><td>2 bytes</td></tr>
<tr><td>5,6</td><td>3 bytes</td></tr>
</tbody></table>

#### [MariaDB 5.3](/kb/en/what-is-mariadb-53/) - [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

<table><tbody><tr><th>Precision</th><th>Storage Requirement</th></tr>
<tr><td>0</td><td>0 bytes</td></tr>
<tr><td>1,2</td><td>1 byte</td></tr>
<tr><td>3,4,5</td><td>2 bytes</td></tr>
<tr><td>6</td><td>3 bytes</td></tr>
</tbody></table>