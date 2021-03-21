# JSON_MERGE

##### MariaDB starting with [10.2.3](/kb/en/mariadb-1023-release-notes/)

JSON functions were added in [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/).

## Syntax

```sql
JSON_MERGE(json_doc, json_doc[, json_doc] ...)
```

## Description

Merges the given JSON documents.

Returns the merged result,or NULL if any argument is NULL.

An error occurs if any of the arguments are not valid JSON documents.

`JSON_MERGE` has been deprecated since [MariaDB 10.2.25](/kb/en/mariadb-10225-release-notes/), [MariaDB 10.3.16](/kb/en/mariadb-10316-release-notes/) and [MariaDB 10.4.5](/kb/en/mariadb-1045-release-notes/). [JSON_MERGE_PATCH](/built-in-functions/special-functions/json-functions/json_merge_patch) is an RFC 7396-compliant replacement, and [JSON_MERGE_PRESERVE](/built-in-functions/special-functions/json-functions/json_merge_preserve) is a synonym.

## Example

```sql
SET @json1 = '[1, 2]';
SET @json2 = '[3, 4]';

SELECT JSON_MERGE(@json1,@json2);
+---------------------------+
| JSON_MERGE(@json1,@json2) |
+---------------------------+
| [1, 2, 3, 4]              |
+---------------------------+
```

## See Also

- [JSON_MERGE_PATCH](/built-in-functions/special-functions/json-functions/json_merge_patch)
- [JSON_MERGE_PRESERVE](/built-in-functions/special-functions/json-functions/json_merge_preserve)