# Binary Literals

Binary literals can be written in one of the following formats: `b'value'`, `B'value'` or `0bvalue`, where `value` is a string composed by `0` and `1` digits.

Binary literals are interpreted as binary strings, and is convenient to represent [VARBINARY](/columns-storage-engines-and-plugins/data-types/string-data-types/varbinary/), [BINARY](/columns-storage-engines-and-plugins/data-types/string-data-types/binary/) or [BIT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/bit/) values.

To convert a binary literal into an integer, just add 0.

## Examples

Printing the value as a binary string:

```sql
SELECT 0b1000001;
+-----------+
| 0b1000001 |
+-----------+
| A         |
+-----------+
```

Converting the same value into a number:

```sql
SELECT 0b1000001+0;
+-------------+
| 0b1000001+0 |
+-------------+
|          65 |
+-------------+
```

## Binary

- [BIN()](/built-in-functions/string-functions/bin/)