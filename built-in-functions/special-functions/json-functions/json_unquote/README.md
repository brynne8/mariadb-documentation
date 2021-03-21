# JSON_UNQUOTE

##### MariaDB starting with [10.2.3](/kb/en/mariadb-1023-release-notes/)

JSON functions were added in [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/).

## Syntax

```sql
JSON_UNQUOTE(val)
```

## Description

Unquotes a JSON value, returning a string, or NULL if the argument is null.

An error will occur if the given value begins and ends with double quotes and is an invalid JSON string literal.

Certain character sequences have special meanings within a string. Usually, a backspace is ignored, but the escape sequences in the table below are recognised by MariaDB, unless the [SQL Mode](/mariadb-administration/variables-and-modes/sql-mode/) is set to NO_BACKSLASH_ESCAPES SQL.

<table><tbody><tr><th>Escape sequence</th><th>Character</th></tr>
<tr><td><code>\"</code></td><td>Double quote (")</td></tr>
<tr><td><code>\b</code></td><td>Backspace</td></tr>
<tr><td><code>\f</code></td><td>Formfeed</td></tr>
<tr><td><code>\n</code></td><td>Newline (linefeed)</td></tr>
<tr><td><code>\r</code></td><td>Carriage return</td></tr>
<tr><td><code>\t</code></td><td>Tab</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">\\</code></td><td>Backslash (\)</td></tr>
<tr><td><code>\uXXXX</code></td><td>UTF-8 bytes for Unicode value XXXX</td></tr>
</tbody></table>

## Examples

```sql
SELECT JSON_UNQUOTE('"Monty"');
+-------------------------+
| JSON_UNQUOTE('"Monty"') |
+-------------------------+
| Monty                   |
+-------------------------+
```

With the default [SQL Mode](/mariadb-administration/variables-and-modes/sql-mode/):

```sql
SELECT JSON_UNQUOTE('Si\bng\ting');
+-----------------------------+
| JSON_UNQUOTE('Si\bng\ting') |
+-----------------------------+
| Sng	ing                   |
+-----------------------------+
```

Setting NO_BACKSLASH_ESCAPES:

```sql
SET @@sql_mode = 'NO_BACKSLASH_ESCAPES';

SELECT JSON_UNQUOTE('Si\bng\ting');
+-----------------------------+
| JSON_UNQUOTE('Si\bng\ting') |
+-----------------------------+
| Si\bng\ting                 |
+-----------------------------+
```