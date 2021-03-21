# Regular Expressions Overview

Regular Expressions allow MariaDB to perform complex pattern matching on a string. In many cases, the simple pattern matching provided by [LIKE](/built-in-functions/string-functions/like) is sufficient. `LIKE` performs two kinds of matches:

- `_` - the underscore, matching a single character
- `%` - the percentage sign, matching any number of characters.

In other cases you may need more control over the returned matches, and will need to use regular expressions.

##### MariaDB starting with [10.0.5](/kb/en/mariadb-1005-release-notes/)

Until [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/), MariaDB used the POSIX 1003.2 compliant regular expression library. The new PCRE library is mostly backwards compatible with what is described below - see the [PCRE Regular Expressions](/kb/en/pcre-regular-expressions/) article for the enhancements made in 10.0.5.

Regular expression matches are performed with the [REGEXP](/built-in-functions/string-functions/regular-expressions-functions/regexp) function. `RLIKE` is a synonym for `REGEXP`.

Comparisons are performed on the byte value, so characters that are treated as equivalent by a collation, but do not have the same byte-value, such as accented characters, could evaluate as unequal. Also note that until [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/), regular expressions were not multi-byte safe, and therefore could produce unexpected results in multi-byte character sets.

Without any special characters, a regular expression match is true if the characters match. The match is case-insensitive, except in the case of BINARY strings.

```sql
SELECT 'Maria' REGEXP 'Maria';
+------------------------+
| 'Maria' REGEXP 'Maria' |
+------------------------+
|                      1 |
+------------------------+

SELECT 'Maria' REGEXP 'maria';
+------------------------+
| 'Maria' REGEXP 'maria' |
+------------------------+
|                      1 |
+------------------------+

SELECT BINARY 'Maria' REGEXP 'maria';
+-------------------------------+
| BINARY 'Maria' REGEXP 'maria' |
+-------------------------------+
|                             0 |
+-------------------------------+
```

Note that the word being matched must match the whole pattern:

```sql
SELECT 'Maria' REGEXP 'Mari';
+-----------------------+
| 'Maria' REGEXP 'Mari' |
+-----------------------+
|                     1 |
+-----------------------+

SELECT 'Mari' REGEXP 'Maria';
+-----------------------+
| 'Mari' REGEXP 'Maria' |
+-----------------------+
|                     0 |
+-----------------------+
```

The first returns true because the pattern "Mari" exists in the expression "Maria". When the order is reversed, the result is false, as the pattern "Maria" does not exist in the expression "Mari"

A match can be performed against more than one word with the `|` character. For example:

```sql
SELECT 'Maria' REGEXP 'Monty|Maria';
+------------------------------+
| 'Maria' REGEXP 'Monty|Maria' |
+------------------------------+
|                            1 |
+------------------------------+
```

## Special Characters

The above examples introduce the syntax, but are not very useful on their own. It's the special characters that give regular expressions their power.

#### ^

`^` matches the beginning of a string (inside square brackets it can also mean NOT - see below):

```sql
SELECT 'Maria' REGEXP '^Ma';
+----------------------+
| 'Maria' REGEXP '^Ma' |
+----------------------+
|                    1 |
+----------------------+
```

#### $

`$` matches the end of a string:

```sql
SELECT 'Maria' REGEXP 'ia$';
+----------------------+
| 'Maria' REGEXP 'ia$' |
+----------------------+
|                    1 |
+----------------------+
```

#### .

`.` matches any single character:

```sql
SELECT 'Maria' REGEXP 'Ma.ia';
+------------------------+
| 'Maria' REGEXP 'Ma.ia' |
+------------------------+
|                      1 |
+------------------------+

SELECT 'Maria' REGEXP 'Ma..ia';
+-------------------------+
| 'Maria' REGEXP 'Ma..ia' |
+-------------------------+
|                       0 |
+-------------------------+
```

#### *

`x*` matches zero or more of a character `x`. In the examples below, it's the `r` character.

```sql
SELECT 'Maria' REGEXP 'Mar*ia';
+-------------------------+
| 'Maria' REGEXP 'Mar*ia' |
+-------------------------+
|                       1 |
+-------------------------+

SELECT 'Maia' REGEXP 'Mar*ia';
+------------------------+
| 'Maia' REGEXP 'Mar*ia' |
+------------------------+
|                      1 |
+------------------------+

SELECT 'Marrria' REGEXP 'Mar*ia';
+---------------------------+
| 'Marrria' REGEXP 'Mar*ia' |
+---------------------------+
|                         1 |
+---------------------------+
```

#### +

`x+` matches one or more of a character `x`. In the examples below, it's the `r` character.

```sql
SELECT 'Maria' REGEXP 'Mar+ia';
+-------------------------+
| 'Maria' REGEXP 'Mar+ia' |
+-------------------------+
|                       1 |
+-------------------------+

SELECT 'Maia' REGEXP 'Mar+ia';
+------------------------+
| 'Maia' REGEXP 'Mar+ia' |
+------------------------+
|                      0 |
+------------------------+

SELECT 'Marrria' REGEXP 'Mar+ia';
+---------------------------+
| 'Marrria' REGEXP 'Mar+ia' |
+---------------------------+
|                         1 |
+---------------------------+
```

#### ?

`x?` matches zero or one of a character `x`. In the examples below, it's the `r` character.

```sql
SELECT 'Maria' REGEXP 'Mar?ia';
+-------------------------+
| 'Maria' REGEXP 'Mar?ia' |
+-------------------------+
|                       1 |
+-------------------------+

SELECT 'Maia' REGEXP 'Mar?ia';
+------------------------+
| 'Maia' REGEXP 'Mar?ia' |
+------------------------+
|                      1 |
+------------------------+

SELECT 'Marrria' REGEXP 'Mar?ia';
+---------------------------+
| 'Marrria' REGEXP 'Mar?ia' |
+---------------------------+
|                         0 |
+---------------------------+
```

#### ()

`(xyz)` - combine a sequence, for example `(xyz)+` or `(xyz)*`

```sql
SELECT 'Maria' REGEXP '(ari)+';
+-------------------------+
| 'Maria' REGEXP '(ari)+' |
+-------------------------+
|                       1 |
+-------------------------+
```

#### {}

`x{n}` and `x{m,n}`
This notation is used to match many instances of the `x`. In the case of `x{n}` the match must be exactly that many times. In the case of `x{m,n}`, the match can occur from `m` to `n` times. For example, to match zero or one instance of the string `ari` (which is identical to `(ari)?`), the following can be used:

```sql
SELECT 'Maria' REGEXP '(ari){0,1}';
+-----------------------------+
| 'Maria' REGEXP '(ari){0,1}' |
+-----------------------------+
|                           1 |
+-----------------------------+
```

#### []

`[xy]` groups characters for matching purposes. For example, to match either the `p` or the `r` character:

```sql
SELECT 'Maria' REGEXP 'Ma[pr]ia';
+---------------------------+
| 'Maria' REGEXP 'Ma[pr]ia' |
+---------------------------+
|                         1 |
+---------------------------+
```

The square brackets also permit a range match, for example, to match any character from a-z, `[a-z]` is used. Numeric ranges are also permitted.

```sql
SELECT 'Maria' REGEXP 'Ma[a-z]ia';
+----------------------------+
| 'Maria' REGEXP 'Ma[a-z]ia' |
+----------------------------+
|                          1 |
+----------------------------+
```

The following does not match, as `r` falls outside of the range `a-p`.

```sql
SELECT 'Maria' REGEXP 'Ma[a-p]ia';
+----------------------------+
| 'Maria' REGEXP 'Ma[a-p]ia' |
+----------------------------+
|                          0 |
+----------------------------+
```

##### ^

The `^` character means does `NOT` match, for example:

```sql
SELECT 'Maria' REGEXP 'Ma[^p]ia';
+---------------------------+
| 'Maria' REGEXP 'Ma[^p]ia' |
+---------------------------+
|                         1 |
+---------------------------+

SELECT 'Maria' REGEXP 'Ma[^r]ia';
+---------------------------+
| 'Maria' REGEXP 'Ma[^r]ia' |
+---------------------------+
|                         0 |
+---------------------------+
```

The `[` and `]` characters on their own can be literally matched inside a `[]` block, without escaping, as long as they immediately match the opening bracket:

```sql
SELECT '[Maria' REGEXP '[[]';
+-----------------------+
| '[Maria' REGEXP '[[]' |
+-----------------------+
|                     1 |
+-----------------------+

SELECT '[Maria' REGEXP '[]]';
+-----------------------+
| '[Maria' REGEXP '[]]' |
+-----------------------+
|                     0 |
+-----------------------+

SELECT ']Maria' REGEXP '[]]';
+-----------------------+
| ']Maria' REGEXP '[]]' |
+-----------------------+
|                     1 |
+-----------------------+

SELECT ']Maria' REGEXP '[]a]';
+------------------------+
| ']Maria' REGEXP '[]a]' |
+------------------------+
|                      1 |
+------------------------+
```

Incorrect  order, so no match:

```sql
SELECT ']Maria' REGEXP '[a]]';
+------------------------+
| ']Maria' REGEXP '[a]]' |
+------------------------+
|                      0 |
+------------------------+
```

The `-` character can also be matched in the same way:

```sql
SELECT '-Maria' REGEXP '[1-10]';
+--------------------------+
| '-Maria' REGEXP '[1-10]' |
+--------------------------+
|                        0 |
+--------------------------+

SELECT '-Maria' REGEXP '[-1-10]';
+---------------------------+
| '-Maria' REGEXP '[-1-10]' |
+---------------------------+
|                         1 |
+---------------------------+
```

#### Word boundaries

The <a undefined>:&lt;:</a> and <a undefined>:&gt;:</a> patterns match the beginning and the end of a word respectively. For example:

```sql
SELECT 'How do I upgrade MariaDB?' REGEXP '[[:<:]]MariaDB[[:>:]]';
+------------------------------------------------------------+
| 'How do I upgrade MariaDB?' REGEXP '[[:<:]]MariaDB[[:>:]]' |
+------------------------------------------------------------+
|                                                          1 |
+------------------------------------------------------------+

SELECT 'How do I upgrade MariaDB?' REGEXP '[[:<:]]Maria[[:>:]]';
+----------------------------------------------------------+
| 'How do I upgrade MariaDB?' REGEXP '[[:<:]]Maria[[:>:]]' |
+----------------------------------------------------------+
|                                                        0 |
+----------------------------------------------------------+
```

#### Character Classes

There are a number of shortcuts to match particular preset character classes. These are matched with the `[:character_class:]` pattern (inside a `[]` set). The following character classes exist:

<table><tbody><tr><th>Character Class</th><th>Description</th></tr>
<tr><td>alnum</td><td>Alphanumeric</td></tr>
<tr><td>alpha</td><td>Alphabetic</td></tr>
<tr><td>blank</td><td>Whitespace</td></tr>
<tr><td>cntrl</td><td>Control characters</td></tr>
<tr><td>digit</td><td>Digits</td></tr>
<tr><td>graph</td><td>Graphic characters</td></tr>
<tr><td>lower</td><td>Lowercase alphabetic</td></tr>
<tr><td>print</td><td>Graphic or space characters</td></tr>
<tr><td>punct</td><td>Punctuation</td></tr>
<tr><td>space</td><td>Space, tab, newline, and carriage return</td></tr>
<tr><td>upper</td><td>Uppercase alphabetic</td></tr>
<tr><td>xdigit</td><td>Hexadecimal digit</td></tr>
</tbody></table>

For example:

```sql
SELECT 'Maria' REGEXP 'Mar[[:alnum:]]*';
+--------------------------------+
| 'Maria' REGEXP 'Mar[:alnum:]*' |
+--------------------------------+
|                              1 |
+--------------------------------+
```

Remember that matches are by default case-insensitive, unless a binary string is used, so the following example, specifically looking for an uppercase, counter-intuitively matches a lowercase character:

```sql
SELECT 'Mari' REGEXP 'Mar[[:upper:]]+';
+---------------------------------+
| 'Mari' REGEXP 'Mar[[:upper:]]+' |
+---------------------------------+
|                               1 |
+---------------------------------+

SELECT BINARY 'Mari' REGEXP 'Mar[[:upper:]]+';
+----------------------------------------+
| BINARY 'Mari' REGEXP 'Mar[[:upper:]]+' |
+----------------------------------------+
|                                      0 |
+----------------------------------------+
```

#### Character Names

There are also number of shortcuts to match particular preset character names. These are matched with the `[.character.]` pattern (inside a `[]` set). The following character classes exist:

<table><tbody><tr><th>Name</th><th>Character</th></tr>
<tr><td>NUL</td><td>0</td></tr>
<tr><td>SOH</td><td>001</td></tr>
<tr><td>STX</td><td>002</td></tr>
<tr><td>ETX</td><td>003</td></tr>
<tr><td>EOT</td><td>004</td></tr>
<tr><td>ENQ</td><td>005</td></tr>
<tr><td>ACK</td><td>006</td></tr>
<tr><td>BEL</td><td>007</td></tr>
<tr><td>alert</td><td>007</td></tr>
<tr><td>BS</td><td>010</td></tr>
<tr><td>backspace</td><td>'\b'</td></tr>
<tr><td>HT</td><td>011</td></tr>
<tr><td>tab</td><td>'\t'</td></tr>
<tr><td>LF</td><td>012</td></tr>
<tr><td>newline</td><td>'\n'</td></tr>
<tr><td>VT</td><td>013</td></tr>
<tr><td>vertical-tab</td><td>'\v'</td></tr>
<tr><td>FF</td><td>014</td></tr>
<tr><td>form-feed</td><td>'\f'</td></tr>
<tr><td>CR</td><td>015</td></tr>
<tr><td>carriage-return</td><td>'\r'</td></tr>
<tr><td>SO</td><td>016</td></tr>
<tr><td>SI</td><td>017</td></tr>
<tr><td>DLE</td><td>020</td></tr>
<tr><td>DC1</td><td>021</td></tr>
<tr><td>DC2</td><td>022</td></tr>
<tr><td>DC3</td><td>023</td></tr>
<tr><td>DC4</td><td>024</td></tr>
<tr><td>NAK</td><td>025</td></tr>
<tr><td>SYN</td><td>026</td></tr>
<tr><td>ETB</td><td>027</td></tr>
<tr><td>CAN</td><td>030</td></tr>
<tr><td>EM</td><td>031</td></tr>
<tr><td>SUB</td><td>032</td></tr>
<tr><td>ESC</td><td>033</td></tr>
<tr><td>IS4</td><td>034</td></tr>
<tr><td>FS</td><td>034</td></tr>
<tr><td>IS3</td><td>035</td></tr>
<tr><td>GS</td><td>035</td></tr>
<tr><td>IS2</td><td>036</td></tr>
<tr><td>RS</td><td>036</td></tr>
<tr><td>IS1</td><td>037</td></tr>
<tr><td>US</td><td>037</td></tr>
<tr><td>space</td><td>' '</td></tr>
<tr><td>exclamation-mark</td><td>'!'</td></tr>
<tr><td>quotation-mark</td><td>'"'</td></tr>
<tr><td>number-sign</td><td>'#'</td></tr>
<tr><td>dollar-sign</td><td>'$'</td></tr>
<tr><td>percent-sign</td><td>'%'</td></tr>
<tr><td>ampersand</td><td>'&amp;'</td></tr>
<tr><td>apostrophe</td><td>'\''</td></tr>
<tr><td>left-parenthesis</td><td>'('</td></tr>
<tr><td>right-parenthesis</td><td>')'</td></tr>
<tr><td>asterisk</td><td>'*'</td></tr>
<tr><td>plus-sign</td><td>'+'</td></tr>
<tr><td>comma</td><td>','</td></tr>
<tr><td>hyphen</td><td>'-'</td></tr>
<tr><td>hyphen-minus</td><td>'-'</td></tr>
<tr><td>period</td><td>'.'</td></tr>
<tr><td>full-stop</td><td>'.'</td></tr>
<tr><td>slash</td><td>'/'</td></tr>
<tr><td>solidus</td><td>'/'</td></tr>
<tr><td>zero</td><td>'0'</td></tr>
<tr><td>one</td><td>'1'</td></tr>
<tr><td>two</td><td>'2'</td></tr>
<tr><td>three</td><td>'3'</td></tr>
<tr><td>four</td><td>'4'</td></tr>
<tr><td>five</td><td>'5'</td></tr>
<tr><td>six</td><td>'6'</td></tr>
<tr><td>seven</td><td>'7'</td></tr>
<tr><td>eight</td><td>'8'</td></tr>
<tr><td>nine</td><td>'9'</td></tr>
<tr><td>colon</td><td>':'</td></tr>
<tr><td>semicolon</td><td>';'</td></tr>
<tr><td>less-than-sign</td><td>'&lt;'</td></tr>
<tr><td>equals-sign</td><td>'='</td></tr>
<tr><td>greater-than-sign</td><td>'&gt;'</td></tr>
<tr><td>question-mark</td><td>'?'</td></tr>
<tr><td>commercial-at</td><td>'@'</td></tr>
<tr><td>left-square-bracket</td><td>'['</td></tr>
<tr><td>backslash</td><td>'<br>'</td></tr>
<tr><td>reverse-solidus</td><td>'<br>'</td></tr>
<tr><td>right-square-bracket</td><td>']'</td></tr>
<tr><td>circumflex</td><td>'^'</td></tr>
<tr><td>circumflex-accent</td><td>'^'</td></tr>
<tr><td>underscore</td><td>'_'</td></tr>
<tr><td>low-line</td><td>'_'</td></tr>
<tr><td>grave-accent</td><td>'`'</td></tr>
<tr><td>left-brace</td><td>'{'</td></tr>
<tr><td>left-curly-bracket</td><td>'{'</td></tr>
<tr><td>vertical-line</td><td>'|'</td></tr>
<tr><td>right-brace</td><td>'}'</td></tr>
<tr><td>right-curly-bracket</td><td>'}'</td></tr>
<tr><td>tilde</td><td>''</td></tr>
<tr><td>DEL</td><td>177</td></tr>
</tbody></table>

For example:

```sql
SELECT '|' REGEXP '[[.vertical-line.]]';
+----------------------------------+
| '|' REGEXP '[[.vertical-line.]]' |
+----------------------------------+
|                                1 |
+----------------------------------+
```

### Combining

The true power of regular expressions is unleashed when the above is combined, to form more complex examples. Regular expression's reputation for complexity stems from the seeming complexity of multiple combined regular expressions, when in reality, it's simply a matter of understanding the characters and how they apply:

The first example fails to match, as while the `Ma` matches, either `i` or `r` only matches once before the `ia` characters at the end.

```sql
SELECT 'Maria' REGEXP 'Ma[ir]{2}ia';
+------------------------------+
| 'Maria' REGEXP 'Ma[ir]{2}ia' |
+------------------------------+
|                            0 |
+------------------------------+
```

This example matches, as either `i` or `r` match exactly twice after the `Ma`, in this case one `r` and one `i`.

```sql
SELECT 'Maria' REGEXP 'Ma[ir]{2}';
+----------------------------+
| 'Maria' REGEXP 'Ma[ir]{2}' |
+----------------------------+
|                          1 |
+----------------------------+
```

### Escaping

With the large number of special characters, care needs to be taken to properly escape characters. Two backslash characters, `<br>` (one for the MariaDB parser, one for the regex library), are required to properly escape a character. For example:

To match the literal `(Ma`:

```sql
SELECT '(Maria)' REGEXP '(Ma';
ERROR 1139 (42000): Got error 'parentheses not balanced' from regexp

SELECT '(Maria)' REGEXP '\(Ma';
ERROR 1139 (42000): Got error 'parentheses not balanced' from regexp

SELECT '(Maria)' REGEXP '\\(Ma';
+--------------------------+
| '(Maria)' REGEXP '\\(Ma' |
+--------------------------+
|                        1 |
+--------------------------+
```

To match `r+`: The first two examples are incorrect, as they match `r` one or more times, not `r+`:

```sql
SELECT 'Mar+ia' REGEXP 'r+';
+----------------------+
| 'Mar+ia' REGEXP 'r+' |
+----------------------+
|                    1 |
+----------------------+

SELECT 'Maria' REGEXP 'r+';
+---------------------+
| 'Maria' REGEXP 'r+' |
+---------------------+
|                   1 |
+---------------------+

SELECT 'Maria' REGEXP 'r\\+';
+-----------------------+
| 'Maria' REGEXP 'r\\+' |
+-----------------------+
|                     0 |
+-----------------------+

SELECT 'Maria' REGEXP 'r+';
+---------------------+
| 'Maria' REGEXP 'r+' |
+---------------------+
|                   1 |
+---------------------+
```