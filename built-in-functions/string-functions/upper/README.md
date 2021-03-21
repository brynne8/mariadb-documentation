# UPPER

## Syntax

```sql
UPPER(str)
```

## Description

Returns the string `str` with all characters changed to uppercase
according to the current character set mapping. The default is latin1
(cp1252 West European).

```sql
SELECT UPPER(surname), givenname FROM users ORDER BY surname;
+----------------+------------+
| UPPER(surname) | givenname  |
+----------------+------------+
| ABEL           | Jacinto    |
| CASTRO         | Robert     |
| COSTA          | Phestos    |
| MOSCHELLA      | Hippolytos |
+----------------+------------+
```

`UPPER()` is ineffective when applied to binary strings ([BINARY](/columns-storage-engines-and-plugins/data-types/string-data-types/binary),
[VARBINARY](/columns-storage-engines-and-plugins/data-types/string-data-types/varbinary), [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob)). The description of 
[LOWER](/built-in-functions/string-functions/lower)() shows how to
perform lettercase conversion of binary strings.