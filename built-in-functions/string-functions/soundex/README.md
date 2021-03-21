# SOUNDEX

## Syntax

```sql
SOUNDEX(str)
```

## Description

Returns a soundex string from <em>`str`</em>. Two strings that sound almost the
same should have identical soundex strings. A standard soundex string is four
characters long, but the `SOUNDEX()` function returns an arbitrarily long
string. You can use `SUBSTRING()` on the result to get a standard soundex
string. All non-alphabetic characters in <em>`str`</em> are ignored. All
international alphabetic characters outside the A-Z range are treated as
vowels.

<strong>Important:</strong> When using SOUNDEX(), you should be aware of the
following limitations:

- This function, as currently implemented, is intended to work well with
  strings that are in the English language only. Strings in other languages may
  not produce reliable results.

## Examples

```sql
SOUNDEX('Hello');
+------------------+
| SOUNDEX('Hello') |
+------------------+
| H400             |
+------------------+
```

```sql
SELECT SOUNDEX('MariaDB');
+--------------------+
| SOUNDEX('MariaDB') |
+--------------------+
| M631               |
+--------------------+
```

```sql
SELECT SOUNDEX('Knowledgebase');
+--------------------------+
| SOUNDEX('Knowledgebase') |
+--------------------------+
| K543212                  |
+--------------------------+
```

```sql
SELECT givenname, surname FROM users WHERE SOUNDEX(givenname) = SOUNDEX("robert");
+-----------+---------+
| givenname | surname |
+-----------+---------+
| Roberto   | Castro  |
+-----------+---------+
```

## See Also

- [SOUNDS LIKE](/built-in-functions/string-functions/sounds-like)()