# Data Types

Data Types in MariaDB



### [Numeric Data Types](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/)

- [Numeric Data Type Overview](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/numeric-data-type-overview/) — Overview and usage of the numeric data types.
- [TINYINT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/tinyint/) — Tiny integer, -128 to 127 signed.
- [BOOLEAN](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/boolean/) — Synonym for TINYINT(1).
- [SMALLINT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/smallint/) — Small integer from -32768 to 32767 signed.
- [MEDIUMINT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/mediumint/) — Medium integer from -8388608 to 8388607 signed.
- [INT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/int/) — Integer from -2147483648 to 2147483647 signed.
- [INTEGER](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/integer/) — Synonym for INT
- [BIGINT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/bigint/) — Large integer.
- [DECIMAL](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/decimal/) — A packed "exact" fixed-point number.
- [DEC, NUMERIC, FIXED](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/dec-numeric-fixed/) — Synonyms for DECIMAL
- [NUMBER](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/number/) — Synonym for DECIMAL in Oracle mode.
- [FLOAT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/float/) — Single-precision floating-point number
- [DOUBLE](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/double/) — Normal-size (double-precision) floating-point number
- [DOUBLE PRECISION](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/double-precision/) — REAL and DOUBLE PRECISION are synonyms for DOUBLE.
- [BIT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/bit/) — Bit field type.
- [Floating-point Accuracy](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/floating-point-accuracy/) — Not all floating-point numbers can be stored with exact precision
- [INT1](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/int1/) — A synonym for TINYINT.
- [INT2](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/int2/) — Synonym for SMALLINT.
- [INT3](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/int3/) — Synonym for MEDIUMINT.
- [INT4](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/int4/) — Synonym for INT.
- [INT8](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/int8/) — Synonym for BIGINT.

### [String Data Types](/columns-storage-engines-and-plugins/data-types/string-data-types/)

- [String Literals](/sql-statements-structure/sql-language-structure/string-literals/) — Strings are sequences of characters and are enclosed with quotes.
- [CHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/char/) — Fixed-length string.
- [VARCHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/varchar/) — Variable-length string.
- [BINARY](/columns-storage-engines-and-plugins/data-types/string-data-types/binary/) — Fixed-length binary byte string.
- [CHAR BYTE](/columns-storage-engines-and-plugins/data-types/string-data-types/char-byte/) — Alias for BINARY.
- [VARBINARY](/columns-storage-engines-and-plugins/data-types/string-data-types/varbinary/) — Variable-length binary byte string.
- [TINYBLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/tinyblob/) — Tiny binary large object up to 255 bytes.
- [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob/) — Binary large object up to 65,535 bytes.
- [BLOB and TEXT Data Types](/columns-storage-engines-and-plugins/data-types/string-data-types/blob-and-text-data-types/) — Binary large object data types and the corresponding TEXT types.
- [MEDIUMBLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/mediumblob/) — Medium binary large object up to 16,777,215 bytes.
- [LONGBLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/longblob/) — Long BLOB holding up to 4GB.
- [TINYTEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/tinytext/) — A TEXT column with a maximum length of 255 characters.
- [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text/) — A TEXT column with a maximum length of 65,535 characters.
- [MEDIUMTEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/mediumtext/) — A TEXT column with a maximum length of 16,777,215 characters.
- [LONGTEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/longtext/) — A TEXT column with a maximum length of 4,294,967,295 characters.
- [INET6](/columns-storage-engines-and-plugins/data-types/string-data-types/inet6/) — For storage of IPv6 addresses.
- [JSON Data Type](/columns-storage-engines-and-plugins/data-types/string-data-types/json-data-type/) — Compatibility data type that is an alias for LONGTEXT.
- [ENUM](/columns-storage-engines-and-plugins/data-types/string-data-types/enum/) — Enumeration, or string object that can have one value chosen from a list of values.
- [SET Data Type](/columns-storage-engines-and-plugins/data-types/string-data-types/set-data-type/) — Set, or string object that can have 0 or more values chosen from a list of values.
- [Supported Character Sets and Collations](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/supported-character-sets-and-collations/) — MariaDB supports the following character sets and collations.
- [Character Sets and Collations](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/) — Setting character set and collation for a language.
- [Data Type Storage Requirements](/columns-storage-engines-and-plugins/data-types/data-type-storage-requirements/) — Storage requirements for the various data types.
- [ROW](/columns-storage-engines-and-plugins/data-types/string-data-types/row/) — Data type for stored procedure variables.

### [Date and Time Data Types](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/)

- [DATE](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/date/) — The date type YYYY-MM-DD.
- [TIME](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/time/) — Time format HH:MM:SS.ssssss
- [DATETIME](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/datetime/) — Date and time combination displayed as YYYY-MM-DD HH:MM:SS.
- [TIMESTAMP](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/timestamp/) — YYYY-MM-DD HH:MM:SS
- [YEAR Data Type](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/year-data-type/) — A four-digit year.
- [Future developments for temporal types](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/future-developments-for-temporal-types/) — My current project is a forecasting application with dates going out to 263...
- [How to define a date in order to import an empty date from a CSV file?](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/how-to-define-a-date-in-order-to-import-an-empty-date-from-a-csv-file/) — I have a CSV file containing amongst other things a couple of date columns....
- [Which datatypes  are supported by MariaDB](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/which-datatypes-are-supported-by-mariadb/) — I would like to know which datatypes are supported by MariaDB. I'm asking s...

### Other Data Types Articles

- [Geometry Types](/sql-statements-structure/geographic-geometric-features/geometry-types/) — Supported geometry types
- [AUTO_INCREMENT](/columns-storage-engines-and-plugins/data-types/auto_increment/) — Automatic increment.
- [Data Type Storage Requirements](/columns-storage-engines-and-plugins/data-types/data-type-storage-requirements/) — Storage requirements for the various data types.
- [AUTO_INCREMENT FAQ](/columns-storage-engines-and-plugins/data-types/auto_increment-faq/) — Frequently-asked questions about auto_increment.
- [NULL Values](/columns-storage-engines-and-plugins/data-types/null-values/) — NULL represents an unknown value.