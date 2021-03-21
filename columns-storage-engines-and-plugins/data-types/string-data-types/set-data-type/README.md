# SET Data Type

## Syntax

```sql
SET('value1','value2',...) [CHARACTER SET charset_name] [COLLATE collation_name]
```

## Description

A set. A string object that can have zero or more values, each of
which must be chosen from the list of values 'value1', 'value2', ... A
SET column can have a maximum of 64 members. SET values are
represented internally as integers.

SET values cannot contain commas.

If a SET contains duplicate values, an error will be returned if [strict mode](/kb/en/sql-mode/#strict-mode) is enabled, or a warning if strict mode is not enabled.

## See Also

- [Character Sets and Collations](/kb/en/character-sets-and-collations/)
- [Data Type Storage Requirements](/columns-storage-engines-and-plugins/data-types/data-type-storage-requirements)