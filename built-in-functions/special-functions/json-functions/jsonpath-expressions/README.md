# JSONPath Expressions

A number of [JSON functions](/built-in-functions/special-functions/json-functions/) accept JSON Path expressions. MariaDB defines this path as follows:

## JSON Path Syntax

```sql
path : ['lax'] '$' [step]*
```

The path starts with an optional <em>path mode</em>.  At the moment, MariaDB supports only the "lax" mode, which is also the mode that is used when it is not explicitly specified.

The `$` symbol represents the context item. The search always starts from the context item; because of that, the path always starts with `$`.

Then, it is followed by zero or more steps, which select element(s) in the JSON document. A step may be one of the following:

- Object member selector
- Array element selector
- Wildcard selector

### Object Member Selector

To select member(s) in a JSON object, one can use one of the following:

- `.memberName` selects the value of the member with name memberName.
- `."memberName"` - the same as above but allows one to select a member with a name that's not a valid identifier (that is, has space, dot, and/or other characters)
- `.*` - selects the values of all members of the object.

If the current item is an array (instead of an object), nothing will be selected.

### Array Element Selector

To select elements of an array, one can use one of the following:

- `[N]` selects element number N in the array. The elements are counted from zero.
- `[*]` selects all elements in the array.

If the current item is an object (instead of an array), nothing will be selected.

### Wildcard

The wildcard step, `**`, recursively selects all child elements of the current element. Both array elements and object members are selected.

The wildcard step must not be the last step in the JSONPath expression. It must be followed by an array or object member selector step.

For example:

```sql
select json_extract(@json_doc, '$**.price');
```

will select all object members in the document that are named `price`,  while

```sql
select json_extract(@json_doc, '$**[2]');
```

will select the second element in each of the arrays present in the document.

## Compatibility

MariaDB's JSONPath syntax supports a subset of JSON Path's definition in the SQL Standard.  The most notable things not supported are the `strict` mode and filters.

MariaDB's JSONPath is close to MySQL's JSONPath.  The wildcard step ( `**` ) is a non-standard extension that  has the same meaning as in MySQL.  The differences between MariaDB and MySQL's JSONPath are: MySQL supports `[last]` and `[M to N]` as array element selectors;  MySQL doesn't allow one to specify the mode explicitly (but uses `lax ` mode implicitly).