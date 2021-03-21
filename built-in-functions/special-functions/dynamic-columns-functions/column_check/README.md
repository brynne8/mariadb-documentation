# COLUMN_CHECK

##### MariaDB starting with [10.0.1](/kb/en/mariadb-1001-release-notes/)

The COLUMN_CHECK function was added in [MariaDB 10.0.1](/kb/en/mariadb-1001-release-notes/).

## Syntax

```sql
COLUMN_CHECK(dyncol_blob);
```

## Description

Check if `dyncol_blob` is a valid packed dynamic columns blob. Return value of 1 means the blob is valid, return value of 0 means it is not.

<strong>Rationale:</strong>
Normally, one works with valid dynamic column blobs. Functions like [COLUMN_CREATE](/built-in-functions/special-functions/dynamic-columns-functions/column_create), [COLUMN_ADD](/built-in-functions/special-functions/dynamic-columns-functions/column_add), [COLUMN_DELETE](/built-in-functions/special-functions/dynamic-columns-functions/column_delete) always return valid dynamic column blobs. However, if a dynamic column blob is accidentally truncated, or transcoded from one character set to another, it will be corrupted. This function can be used to check if a value in a blob field is a valid dynamic column blob.