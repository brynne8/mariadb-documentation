# DOUBLE PRECISION

## Syntax

```sql
DOUBLE PRECISION[(M,D)] [SIGNED | UNSIGNED | ZEROFILL]
REAL[(M,D)] [SIGNED | UNSIGNED | ZEROFILL]
```

## Description

REAL and DOUBLE PRECISION are synonyms for [DOUBLE](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/double/).

Exception: If the REAL_AS_FLOAT [SQL mode](/kb/en/sql_mode/) is enabled, 
REAL is a synonym for [FLOAT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/float/) rather than 
[DOUBLE](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/double/).