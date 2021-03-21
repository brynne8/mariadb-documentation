# BINARY

This page describes the BINARY data type. For details about the operator, see [Binary Operator](/built-in-functions/string-functions/binary-operator).

## Syntax

```sql
BINARY(M)
```

## Description

The `BINARY` type is similar to the [CHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/char) type, but stores binary
byte strings rather than non-binary character strings. `M` represents the
column length in bytes.

It contains no character set, and comparison and sorting are based on the numeric value of the bytes.

If the maximum length is exceeded, and [SQL strict mode](/mariadb-administration/variables-and-modes/sql-mode) is not enabled , the extra characters will be dropped with a warning. If strict mode is enabled, an error will occur.

BINARY values are right-padded with `0x00` (the zero byte) to the specified length when inserted. The padding is <em>not</em> removed on select, so this needs to be taken into account when sorting and comparing, where all bytes are significant. The zero byte, `0x00` is less than a space for comparison purposes.

## Examples

Inserting too many characters, first with strict mode off, then with it on:

```sql
CREATE TABLE bins (a BINARY(10));

INSERT INTO bins VALUES('12345678901');
Query OK, 1 row affected, 1 warning (0.04 sec)

SELECT * FROM bins;
+------------+
| a          |
+------------+
| 1234567890 |
+------------+

SET sql_mode='STRICT_ALL_TABLES';

INSERT INTO bins VALUES('12345678901');
ERROR 1406 (22001): Data too long for column 'a' at row 1
```

Sorting is performed with the byte value:

```sql
TRUNCATE bins;

INSERT INTO bins VALUES('A'),('B'),('a'),('b');

SELECT * FROM bins ORDER BY a;
+------+
| a    |
+------+
| A    |
| B    |
| a    |
| b    |
+------+
```

Using [CAST](/built-in-functions/string-functions/cast) to sort as a [CHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/char) instead:

```sql
SELECT * FROM bins ORDER BY CAST(a AS CHAR);
+------+
| a    |
+------+
| a    |
| A    |
| b    |
| B    |
+------+
```

The field is a BINARY(10), so padding of two '\0's are inserted, causing comparisons that don't take this into account to fail:

```sql
TRUNCATE bins;

INSERT INTO bins VALUES('12345678');

SELECT a = '12345678', a = '12345678\0\0' from bins;
+----------------+--------------------+
| a = '12345678' | a = '12345678\0\0' |
+----------------+--------------------+
|              0 |                  1 |
+----------------+--------------------+
```

## See Also

- [CHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/char)
- [Data Type Storage Requirements](/columns-storage-engines-and-plugins/data-types/data-type-storage-requirements)