# mroonga_escape

## Syntax

```sql
mroonga_escape (string [,special_characters])
```

- `string` - required parameter specifying the text you want to escape
- `special_characters` - optional parameter specifying the characters to escape

## Description

`mroonga_escape` is a [user-defined function](/programming-customizing-mariadb/user-defined-functions/) (UDF) included with the [Mroonga storage engine](/columns-storage-engines-and-plugins/storage-engines/mroonga/), used for escaping a string. See [Creating Mroonga User-Defined Functions](/columns-storage-engines-and-plugins/storage-engines/mroonga/mroonga-user-defined-functions/creating-mroonga-user-defined-functions/) for details on creating this UDF if required.

If no `special_characters` parameter is provided, by default `+-&lt;&gt;*()":` are escaped.

Returns the escaped string.

## Example

```sql
SELECT mroonga_escape("+-<>~*()\"\:");
'\\+\\-\\<\\>\\~\\*\\(\\)\\"\\:
```

## See Also

- [Creating Mroonga User-Defined Functions](/columns-storage-engines-and-plugins/storage-engines/mroonga/mroonga-user-defined-functions/creating-mroonga-user-defined-functions/)