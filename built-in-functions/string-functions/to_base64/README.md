# TO_BASE64

##### MariaDB starting with [10.0.5](/kb/en/mariadb-1005-release-notes/)

The TO_BASE64() function was introduced in [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/).

## Syntax

```sql
TO_BASE64(str)
```

## Description

Converts the string argument `str` to its base-64 encoded form, returning the result as a character string in the connection character set and collation.

The argument `str` will be converted to string first if it is not a string. A NULL argument will return a NULL result.

The reverse function, [FROM_BASE64()](/built-in-functions/string-functions/from_base64), decodes an encoded base-64 string.

There are a numerous different methods to base-64 encode a string. The following are used by MariaDB and MySQL:

- Alphabet value 64 is encoded as '+'.
- Alphabet value 63 is encoded as '/'.
- Encoding output is made up of groups of four printable characters, with each three bytes of data encoded using four characters. If the final group is not complete, it is padded with '=' characters to make up a length of four.
- To divide long output, a newline is added after every 76 characters.
- Decoding will recognize and ignore newlines, carriage returns, tabs, and spaces.

## Examples

```sql
SELECT TO_BASE64('Maria');
+--------------------+
| TO_BASE64('Maria') |
+--------------------+
| TWFyaWE=           |
+--------------------+
```