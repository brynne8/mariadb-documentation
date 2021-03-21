# JSON_MERGE_PRESERVE

##### MariaDB starting with [10.2.25](/kb/en/mariadb-10225-release-notes/)

`JSON_MERGE_PRESERVE` was introduced in [MariaDB 10.2.25](/kb/en/mariadb-10225-release-notes/), [MariaDB 10.3.16](/kb/en/mariadb-10316-release-notes/) and [MariaDB 10.4.5](/kb/en/mariadb-1045-release-notes/).

## Syntax

```sql
JSON_MERGE_PRESERVE(json_doc, json_doc[, json_doc] ...)
```

## Description

Merges the given JSON documents, returning the merged result, or NULL if any argument is NULL.

`JSON_MERGE_PRESERVE` was introduced in [MariaDB 10.2.25](/kb/en/mariadb-10225-release-notes/), [MariaDB 10.3.16](/kb/en/mariadb-10316-release-notes/) and [MariaDB 10.4.5](/kb/en/mariadb-1045-release-notes/) as a synonym for [JSON_MERGE](/built-in-functions/special-functions/json-functions/json_merge/), which has been deprecated.

## Example

```sql
SET @json1 = '[1, 2]';
SET @json2 = '[2, 3]';
SELECT JSON_MERGE_PATCH(@json1,@json2),JSON_MERGE_PRESERVE(@json1,@json2);
+---------------------------------+------------------------------------+
| JSON_MERGE_PATCH(@json1,@json2) | JSON_MERGE_PRESERVE(@json1,@json2) |
+---------------------------------+------------------------------------+
| [2, 3]                          | [1, 2, 2, 3]                       |
+---------------------------------+------------------------------------+
```

## See Also

- [JSON_MERGE_PATCH](/built-in-functions/special-functions/json-functions/json_merge_patch/)