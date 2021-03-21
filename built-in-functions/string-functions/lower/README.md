# LOWER

## Syntax

```sql
LOWER(str)
```

## Description

Returns the string `str` with all characters changed to lowercase
according to the current character set mapping. The default is latin1
(cp1252 West European).

## Examples

```sql
 SELECT LOWER('QUADRATICALLY');
+------------------------+
| LOWER('QUADRATICALLY') |
+------------------------+
| quadratically          |
+------------------------+
```

`LOWER()` (and [UPPER](/built-in-functions/string-functions/upper/)()) are ineffective when applied to binary
strings ([BINARY](/columns-storage-engines-and-plugins/data-types/string-data-types/binary/), [VARBINARY](/columns-storage-engines-and-plugins/data-types/string-data-types/varbinary/), [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob/)). 
To perform lettercase conversion, [CONVERT](/built-in-functions/string-functions/convert/) the string to a non-binary string:

```sql
SET @str = BINARY 'North Carolina';

SELECT LOWER(@str), LOWER(CONVERT(@str USING latin1));
+----------------+-----------------------------------+
| LOWER(@str)    | LOWER(CONVERT(@str USING latin1)) |
+----------------+-----------------------------------+
| North Carolina | north carolina                    |
+----------------+-----------------------------------+
```