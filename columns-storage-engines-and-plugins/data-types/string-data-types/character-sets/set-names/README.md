# SET NAMES

## Syntax

```sql
SET NAMES {'charset_name'
    [COLLATE 'collation_name'] | DEFAULT}
```

## Description

Sets the [character_set_client](/kb/en/server-system-variables/#character_set_client), [character_set_connection](/kb/en/server-system-variables/#character_set_connection), [character_set_results](/kb/en/server-system-variables/#character_set_results) and, implicitly, the [collation_connection](/kb/en/server-system-variables/#collation_connection) session system variables to the specified character set and collation.

This determines which [character set](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets) the client will use to send statements to the server, and the server will use for sending results back to the client.

`ucs2`, `utf16`, and `utf32` are not valid character sets for `SET NAMES`, as they cannot be used as client character sets.

The collation clause is optional. If not defined (or if `DEFAULT` is specified), the [default collation for the character set](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/supported-character-sets-and-collations) will be used.

Quotes are optional for the character set or collation clauses.

## Examples

```sql
SELECT VARIABLE_NAME, SESSION_VALUE 
  FROM INFORMATION_SCHEMA.SYSTEM_VARIABLES WHERE 
  VARIABLE_NAME LIKE 'character_set_c%' OR 
  VARIABLE_NAME LIKE 'character_set_re%' OR 
  VARIABLE_NAME LIKE 'collation_c%';
+--------------------------+-----------------+
| VARIABLE_NAME            | SESSION_VALUE   |
+--------------------------+-----------------+
| CHARACTER_SET_RESULTS    | utf8            |
| CHARACTER_SET_CONNECTION | utf8            |
| CHARACTER_SET_CLIENT     | utf8            |
| COLLATION_CONNECTION     | utf8_general_ci |
+--------------------------+-----------------+

SET NAMES big5;

SELECT VARIABLE_NAME, SESSION_VALUE 
  FROM INFORMATION_SCHEMA.SYSTEM_VARIABLES WHERE 
  VARIABLE_NAME LIKE 'character_set_c%' OR 
  VARIABLE_NAME LIKE 'character_set_re%' OR 
  VARIABLE_NAME LIKE 'collation_c%';
+--------------------------+-----------------+
| VARIABLE_NAME            | SESSION_VALUE   |
+--------------------------+-----------------+
| CHARACTER_SET_RESULTS    | big5            |
| CHARACTER_SET_CONNECTION | big5            |
| CHARACTER_SET_CLIENT     | big5            |
| COLLATION_CONNECTION     | big5_chinese_ci |
+--------------------------+-----------------+

SET NAMES 'latin1' COLLATE 'latin1_bin';

SELECT VARIABLE_NAME, SESSION_VALUE 
  FROM INFORMATION_SCHEMA.SYSTEM_VARIABLES WHERE 
  VARIABLE_NAME LIKE 'character_set_c%' OR 
  VARIABLE_NAME LIKE 'character_set_re%' OR 
  VARIABLE_NAME LIKE 'collation_c%';
+--------------------------+---------------+
| VARIABLE_NAME            | SESSION_VALUE |
+--------------------------+---------------+
| CHARACTER_SET_RESULTS    | latin1        |
| CHARACTER_SET_CONNECTION | latin1        |
| CHARACTER_SET_CLIENT     | latin1        |
| COLLATION_CONNECTION     | latin1_bin    |
+--------------------------+---------------+

SET NAMES DEFAULT;

SELECT VARIABLE_NAME, SESSION_VALUE 
  FROM INFORMATION_SCHEMA.SYSTEM_VARIABLES WHERE 
  VARIABLE_NAME LIKE 'character_set_c%' OR 
  VARIABLE_NAME LIKE 'character_set_re%' OR 
  VARIABLE_NAME LIKE 'collation_c%';
+--------------------------+-------------------+
| VARIABLE_NAME            | SESSION_VALUE     |
+--------------------------+-------------------+
| CHARACTER_SET_RESULTS    | latin1            |
| CHARACTER_SET_CONNECTION | latin1            |
| CHARACTER_SET_CLIENT     | latin1            |
| COLLATION_CONNECTION     | latin1_swedish_ci |
+--------------------------+-------------------+
```

## See Also

- [SET CHARACTER SET](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/set-character-set)
- [Character Sets and Collations](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets)