# VARBINARY

## Syntax

```sql
VARBINARY(M)
```

## Description

The VARBINARY type is similar to the [VARCHAR](/kb/en/sql_language-data_types-varchar/) type, but stores binary byte strings rather than non-binary character strings. `M` represents the maximum column length in bytes.

It contains no [character set](/kb/en/data-types-character-sets-and-collations/), and comparison and sorting are based on the numeric value of the bytes.

If the maximum length is exceeded, and [SQL strict mode](/mariadb-administration/variables-and-modes/sql-mode/) is not enabled , the extra characters will be dropped with a warning. If strict mode is enabled, an error will occur.

Unlike [BINARY](/columns-storage-engines-and-plugins/data-types/string-data-types/binary/) values, VARBINARYs are not right-padded when inserting.

### Oracle Mode

##### MariaDB starting with [10.3](/kb/en/what-is-mariadb-103/)

In [Oracle mode from MariaDB 10.3](/kb/en/sql_modeoracle-from-mariadb-103/#synonyms-for-basic-sql-types), `RAW` is a synonym for `VARBINARY`.

## Examples

Inserting too many characters, first with strict mode off, then with it on:

```sql
CREATE TABLE varbins (a VARBINARY(10));

INSERT INTO varbins VALUES('12345678901');
Query OK, 1 row affected, 1 warning (0.04 sec)

SELECT * FROM varbins;
+------------+
| a          |
+------------+
| 1234567890 |
+------------+

SET sql_mode='STRICT_ALL_TABLES';

INSERT INTO varbins VALUES('12345678901');
ERROR 1406 (22001): Data too long for column 'a' at row 1
```

Sorting is performed with the byte value:

```sql
TRUNCATE varbins;

INSERT INTO varbins VALUES('A'),('B'),('a'),('b');

SELECT * FROM varbins ORDER BY a;
+------+
| a    |
+------+
| A    |
| B    |
| a    |
| b    |
+------+
```

Using [CAST](/built-in-functions/string-functions/cast/) to sort as a [CHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/char/) instead:

```sql
SELECT * FROM varbins ORDER BY CAST(a AS CHAR);
+------+
| a    |
+------+
| a    |
| A    |
| b    |
| B    |
+------+
```

## See Also

- [VARCHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/varchar/)
- [Data Type Storage Requirements](/columns-storage-engines-and-plugins/data-types/data-type-storage-requirements/)
- [Oracle mode from MariaDB 10.3](/kb/en/sql_modeoracle-from-mariadb-103/#synonyms-for-basic-sql-types)