# String Literals

Strings are sequences of characters and are enclosed with quotes.

The syntax is:

```sql
[_charset_name]'string' [COLLATE collation_name]
```

For example:

```sql
'The MariaDB Foundation'
_utf8 'Foundation' COLLATE utf8_unicode_ci;
```

Strings can either be enclosed in single quotes or in double quotes (the same character must be used to both open and close the string).

The ANSI SQL-standard does not permit double quotes for enclosing strings, and although MariaDB does by default, if the MariaDB server has enabled the [ANSI_QUOTES_SQL](/kb/en/sql-mode/#ansi_quotes) [SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode), double quotes will be treated as being used for [identifiers](/sql-statements-structure/sql-language-structure/identifier-names) instead of strings.

Strings that are next to each other are automatically concatenated. For example:

```sql
'The ' 'MariaDB ' 'Foundation'
```

and

```sql
'The MariaDB Foundation'
```

are equivalent.

The `\` (backslash character) is used to escape characters (unless the [SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode) hasn't been set to [NO_BACKSLASH_ESCAPES](/kb/en/sql-mode/#no_backslash_escapes)). For example:

```sql
'MariaDB's new features'
```

is not a valid string because of the single quote in the middle of the string, which is treated as if it closes the string, but is actually meant as part of the string, an apostrophe. The backslash character helps in situations like this:

```sql
'MariaDB\'s new features'
```

is now a valid string, and if displayed, will appear without the backslash.

```sql
SELECT 'MariaDB\'s new features';
+------------------------+
| MariaDB's new features |
+------------------------+
| MariaDB's new features |
+------------------------+
```

Another way to escape the quoting character is repeating it twice:

```sql
SELECT 'I''m here', """Double""";
+----------+----------+
| I'm here | "Double" |
+----------+----------+
| I'm here | "Double" |
+----------+----------+
```

## Escape sequences

There are other escape sequences also. Here is a full list:

<table><tbody><tr><th>Escape sequence</th><th>Character</th></tr>
<tr><td><code>\0</code></td><td>ASCII NUL (0x00).</td></tr>
<tr><td><code>\'</code></td><td>Single quote (“'”).</td></tr>
<tr><td><code>\"</code></td><td>Double quote (“"”).</td></tr>
<tr><td><code>\b</code></td><td>Backspace.</td></tr>
<tr><td><code>\n</code></td><td>Newline, or linefeed,.</td></tr>
<tr><td><code>\r</code></td><td>Carriage return.</td></tr>
<tr><td><code>\t</code></td><td>Tab.</td></tr>
<tr><td><code>\Z</code></td><td>ASCII 26 (Control+Z). See note following the table.</td></tr>
<tr><td><code>\\</code></td><td>Backslash (“\”).</td></tr>
<tr><td><code>\%</code></td><td>“%” character. See note following the table.</td></tr>
<tr><td><code>\_</code></td><td>A “_” character. See note following the table.</td></tr>
</tbody></table>

Escaping the `%` and `_` characters can be necessary when using the [LIKE](/built-in-functions/string-functions/like) operator, which treats them as special characters.

The ASCII 26 character (`\Z`) needs to be escaped when included in a batch file which needs to be executed in Windows. The reason is that ASCII 26, in Windows, is the end of file (EOF).

Backslash (`\`), if not used as an escape character, must always be escaped. When followed by a character that is not in the above table, backslashes will simply be ignored.