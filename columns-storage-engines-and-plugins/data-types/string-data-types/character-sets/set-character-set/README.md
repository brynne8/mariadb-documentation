# SET CHARACTER SET

## Syntax

```sql
SET {CHARACTER SET | CHARSET}
    {charset_name | DEFAULT}
```

## Description

Sets the <a undefined>character_set_client</a> and <a undefined>character_set_results</a> session system variables to the specified character set and  <a undefined>collation_connection</a> to the value of <a undefined>collation_database</a>, which implicitly sets <a undefined>character_set_connection</a> to the value of <a undefined>character_set_database</a>.

This maps all strings sent between the current client and the server with the given mapping.

## See Also

- [SET NAMES](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/set-names)