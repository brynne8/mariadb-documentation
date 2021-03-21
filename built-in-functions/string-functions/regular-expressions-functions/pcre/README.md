# Perl Compatible Regular Expressions (PCRE) Documentation

## PCRE Versions

<table><tbody><tr><th>PCRE Version</th><th>Introduced</th><th>Maturity</th></tr>
<tr><td><a href="http://www.pcre.org/">PCRE2</a> 10.34</td><td><a href="/kb/en/mariadb-1051-release-notes/">MariaDB 10.5.1</a></td><td>Stable</td></tr>
<tr><td>PCRE 8.43</td><td><a href="/kb/en/mariadb-10139-release-notes/">MariaDB 10.1.39</a></td><td>Stable</td></tr>
<tr><td>PCRE 8.42</td><td><a href="/kb/en/mariadb-10215-release-notes/">MariaDB 10.2.15</a>, <a href="/kb/en/mariadb-10133-release-notes/">MariaDB 10.1.33</a>, <a href="/kb/en/mariadb-10035-release-notes/">MariaDB 10.0.35</a></td><td>Stable</td></tr>
<tr><td>PCRE 8.41</td><td><a href="/kb/en/mariadb-1028-release-notes/">MariaDB 10.2.8</a>, <a href="/kb/en/mariadb-10126-release-notes/">MariaDB 10.1.26</a>, <a href="/kb/en/mariadb-10032-release-notes/">MariaDB 10.0.32</a></td><td>Stable</td></tr>
<tr><td>PCRE 8.40</td><td><a href="/kb/en/mariadb-1025-release-notes/">MariaDB 10.2.5</a>, <a href="/kb/en/mariadb-10122-release-notes/">MariaDB 10.1.22</a>, <a href="/kb/en/mariadb-10030-release-notes/">MariaDB 10.0.30</a></td><td>Stable</td></tr>
<tr><td>PCRE 8.39</td><td><a href="/kb/en/mariadb-10115-release-notes/">MariaDB 10.1.15</a>, <a href="/kb/en/mariadb-10026-release-notes/">MariaDB 10.0.26</a></td><td>Stable</td></tr>
<tr><td>PCRE 8.38</td><td><a href="/kb/en/mariadb-10110-release-notes/">MariaDB 10.1.10</a>, <a href="/kb/en/mariadb-10023-release-notes/">MariaDB 10.0.23</a></td><td>Stable</td></tr>
<tr><td>PCRE 8.37</td><td><a href="/kb/en/mariadb-1015-release-notes/">MariaDB 10.1.5</a>, <a href="/kb/en/mariadb-10018-release-notes/">MariaDB 10.0.18</a></td><td>Stable</td></tr>
<tr><td>PCRE 8.36</td><td><a href="/kb/en/mariadb-1012-release-notes/">MariaDB 10.1.2</a>, <a href="/kb/en/mariadb-10015-release-notes/">MariaDB 10.0.15</a></td><td>Stable</td></tr>
<tr><td>PCRE 8.35</td><td><a href="/kb/en/mariadb-1010-release-notes/">MariaDB 10.1.0</a>, <a href="/kb/en/mariadb-10012-release-notes/">MariaDB 10.0.12</a></td><td>Stable</td></tr>
<tr><td>PCRE 8.34</td><td><a href="/kb/en/mariadb-1008-release-notes/">MariaDB 10.0.8</a></td><td>Stable</td></tr>
</tbody></table>

## PCRE Enhancements

[MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/) switched to the PCRE library, which significantly improved the power of the [REGEXP/RLIKE](/built-in-functions/string-functions/regular-expressions-functions/regexp) operator.

The switch to PCRE added a number of features, including recursive patterns, named capture, look-ahead and look-behind assertions, non-capturing groups, non-greedy quantifiers, Unicode character properties, extended syntax for characters and character classes, multi-line matching, and many other.

Additionally, [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/) introduced three new functions that work with regular expressions: [REGEXP_REPLACE()](/built-in-functions/string-functions/regular-expressions-functions/regexp_replace), [REGEXP_INSTR()](/built-in-functions/string-functions/regular-expressions-functions/regexp_instr) and [REGEXP_SUBSTR()](/built-in-functions/string-functions/regular-expressions-functions/regexp_substr).

Also, REGEXP/RLIKE, and the new functions, now work correctly with all multi-byte [character sets](/kb/en/data-types-character-sets-and-collations/) supported by MariaDB, including East-Asian character sets (big5, gb2313, gbk, eucjp, eucjpms, cp932, ujis, euckr), and Unicode character sets (utf8, utf8mb4, ucs2, utf16, utf16le, utf32). In earlier versions of MariaDB (and all MySQL versions) REGEXP/RLIKE works correctly only with 8-bit character sets.

## New Regular Expression Functions

- [REGEXP_REPLACE(subject, pattern, replace)](/built-in-functions/string-functions/regular-expressions-functions/regexp_replace) - Replaces all occurrences of a pattern.
- [REGEXP_INSTR(subject, pattern)](/built-in-functions/string-functions/regular-expressions-functions/regexp_instr) - Position of the first appearance of a regex .
- [REGEXP_SUBSTR(subject,pattern)](/built-in-functions/string-functions/regular-expressions-functions/regexp_substr) - Returns the matching part of a string.

See the individual articles for more details and examples.

## PCRE Syntax

In most cases PCRE is backward compatible with the old POSIX 1003.2 compliant regexp library (see [Regular Expressions Overview](/built-in-functions/string-functions/regular-expressions-functions/regular-expressions-overview)), so you won't need to change your applications that use SQL queries with the REGEXP/RLIKE predicate.

[MariaDB 10.0.11](/kb/en/mariadb-10011-release-notes/) introduced the [default_regex_flags](/kb/en/server-system-variables/#default_regex_flags) variable to address the remaining compatibilities between PCRE and the old regex library.

This section briefly describes the most important extended PCRE features. For more details please refer to the documentation on the [PCRE site](http://www.pcre.org/), or to the documentation which is bundled in the /pcre/doc/html/ directory of a MariaDB sources distribution. The pages pcresyntax.html and pcrepattern.html should be a good start. [Regular-Expressions.Info](http://www.regular-expressions.info/tutorial.html) is another good resource to learn about PCRE and regular expressions generally.

### Special Characters

PCRE supports the following escape sequences to match special characters:

<table><tbody><tr><th>Sequence</th><th>Description</th></tr>
<tr><td>\a</td><td>0x07 (BEL)</td></tr>
<tr><td>\cx</td><td>"control-x", where x is any ASCII character</td></tr>
<tr><td>\e</td><td>0x1B (escape)</td></tr>
<tr><td>\f</td><td>0x0C (form feed)</td></tr>
<tr><td>\n</td><td>0x0A (newline)</td></tr>
<tr><td>\r</td><td>0x0D (carriage return)</td></tr>
<tr><td>\t</td><td>0x09 (TAB)</td></tr>
<tr><td>\ddd</td><td>character with octal code ddd</td></tr>
<tr><td>\xhh</td><td>character with hex code hh</td></tr>
<tr><td>\x{hhh..}</td><td>character with hex code hhh..</td></tr>
</tbody></table>

Note, the backslash characters (here, and in all examples in the sections below) must be escaped with another backslash, unless you're using the [SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode) `NO_BACKSLASH_ESCAPES`.

This example tests if a character has hex code 0x61:

```sql
SELECT 'a' RLIKE '\\x{61}';
-> 1
```

### Character Classes

PCRE supports the standard POSIX character classes such as `alnum`, `alpha`, `blank`, `cntrl`, `digit`, `graph`, `lower`, `print`, `punct`, `space`, `upper`, `xdigit`, with the following additional classes:

<table><tbody><tr><th>Class</th><th>Description</th></tr>
<tr><td>ascii</td><td>any ASCII character (0x00..0x7F)</td></tr>
<tr><td>word</td><td>any "word" character (a letter, a digit, or an underscore)</td></tr>
</tbody></table>

This example checks if the string consists of ASCII characters only:

```sql
SELECT 'abc' RLIKE '^[[:ascii:]]+$';
-> 1
```

### Generic Character Types

Generic character types complement the POSIX character classes and serve to simplify writing patterns:

<table><tbody><tr><th>Class</th><th>Description</th></tr>
<tr><td>\d</td><td>a decimal digit (same as [:digit:])</td></tr>
<tr><td>\D</td><td>a character that is not a decimal digit</td></tr>
<tr><td>\h</td><td>a horizontal white space character</td></tr>
<tr><td>\H</td><td>a character that is not a horizontal white space character</td></tr>
<tr><td>\N</td><td>a character that is not a new line</td></tr>
<tr><td>\R</td><td>a newline sequence</td></tr>
<tr><td>\s</td><td>a white space character</td></tr>
<tr><td>\S</td><td>a character that is not a white space character</td></tr>
<tr><td>\v</td><td>a vertical white space character</td></tr>
<tr><td>\V</td><td>a character that is not a vertical white space character</td></tr>
<tr><td>\w</td><td>a "word" character (same as [:word:])</td></tr>
<tr><td>\W</td><td>a "non-word" character</td></tr>
</tbody></table>

This example checks if the string consists of "word" characters only:

```sql
SELECT 'abc' RLIKE '^\\w+$';
-> 1
```

### Unicode Character Properties

`\p{xx}` is a character with the `xx` property, and `\P{xx}` is a character without the `xx` property.

The property names represented by `xx` above are limited to the Unicode script names, the general category properties, and "Any", which matches any character (including newline). Those that are not part of an identified script are lumped together as "Common".

#### General Category Properties For \p and \P

<table><tbody><tr><th>Property</th><th>Description</th></tr>
<tr><td>C</td><td>Other</td></tr>
<tr><td>Cc</td><td>Control</td></tr>
<tr><td>Cf</td><td>Format</td></tr>
<tr><td>Cn</td><td>Unassigned</td></tr>
<tr><td>Co</td><td>Private use</td></tr>
<tr><td>Cs</td><td>Surrogate</td></tr>
<tr><td>L</td><td>Letter</td></tr>
<tr><td>Ll</td><td>Lower case letter</td></tr>
<tr><td>Lm</td><td>Modifier letter</td></tr>
<tr><td>Lo</td><td>Other letter</td></tr>
<tr><td>Lt</td><td>Title case letter</td></tr>
<tr><td>Lu</td><td>Upper case letter</td></tr>
<tr><td>L&amp;</td><td>Ll, Lu, or Lt</td></tr>
<tr><td>M</td><td>Mark</td></tr>
<tr><td>Mc</td><td>Spacing mark</td></tr>
<tr><td>Me</td><td>Enclosing mark</td></tr>
<tr><td>Mn</td><td>Non-spacing mark</td></tr>
<tr><td>N</td><td>Number</td></tr>
<tr><td>Nd</td><td>Decimal number</td></tr>
<tr><td>Nl</td><td>Letter number</td></tr>
<tr><td>No</td><td>Other number</td></tr>
<tr><td>P</td><td>Punctuation</td></tr>
<tr><td>Pc</td><td>Connector punctuation</td></tr>
<tr><td>Pd</td><td>Dash punctuation</td></tr>
<tr><td>Pe</td><td>Close punctuation</td></tr>
<tr><td>Pf</td><td>Final punctuation</td></tr>
<tr><td>Pi</td><td>Initial punctuation</td></tr>
<tr><td>Po</td><td>Other punctuation</td></tr>
<tr><td>Ps</td><td>Open punctuation</td></tr>
<tr><td>S</td><td>Symbol</td></tr>
<tr><td>Sc</td><td>Currency symbol</td></tr>
<tr><td>Sk</td><td>Modifier symbol</td></tr>
<tr><td>Sm</td><td>Mathematical symbol</td></tr>
<tr><td>So</td><td>Other symbol</td></tr>
<tr><td>Z</td><td>Separator</td></tr>
<tr><td>Zl</td><td>Line separator</td></tr>
<tr><td>Zp</td><td>Paragraph separator</td></tr>
<tr><td>Zs</td><td>Space separator</td></tr>
</tbody></table>

This example checks if the string consists only of characters with property N (number):

```sql
SELECT '1¼①' RLIKE '^\\p{N}+$';
-> 1
```

#### Special Category Properties For \p and \P

<table><tbody><tr><th>Property</th><th>Description</th></tr>
<tr><td>Xan</td><td>Alphanumeric: union of properties L and N</td></tr>
<tr><td>Xps</td><td>POSIX space: property Z or tab, NL, VT, FF, CR</td></tr>
<tr><td>Xsp</td><td>Perl space: property Z or tab, NL, FF, CR</td></tr>
<tr><td>Xuc</td><td>A character than can be represented by a Universal Character Name</td></tr>
<tr><td>Xwd</td><td>Perl word: property Xan or underscore</td></tr>
</tbody></table>

The property `Xuc` matches any character that can be represented by a Universal Character Name (in C++ and other programming languages). These include `$`, `@`, ```, and all characters with Unicode code points greater than `U+00A0`, excluding the surrogates `U+D800`..`U+DFFF`.

#### Script Names For \p and \P

Arabic, Armenian, Avestan, Balinese, Bamum, Batak, Bengali, Bopomofo, Brahmi, Braille, Buginese, Buhid, Canadian_Aboriginal, Carian, Chakma, Cham, Cherokee, Common, Coptic, Cuneiform, Cypriot, Cyrillic, Deseret, Devanagari, Egyptian_Hieroglyphs, Ethiopic, Georgian, Glagolitic, Gothic, Greek, Gujarati, Gurmukhi, Han, Hangul, Hanunoo, Hebrew, Hiragana, Imperial_Aramaic, Inherited, Inscriptional_Pahlavi, Inscriptional_Parthian, Javanese, Kaithi, Kannada, Katakana, Kayah_Li, Kharoshthi, Khmer, Lao, Latin, Lepcha, Limbu, Linear_B, Lisu, Lycian, Lydian, Malayalam, Mandaic, Meetei_Mayek, Meroitic_Cursive, Meroitic_Hieroglyphs, Miao, Mongolian, Myanmar, New_Tai_Lue, Nko, Ogham, Old_Italic, Old_Persian, Old_South_Arabian, Old_Turkic, Ol_Chiki, Oriya, Osmanya, Phags_Pa, Phoenician, Rejang, Runic, Samaritan, Saurashtra, Sharada, Shavian, Sinhala, Sora_Sompeng, Sundanese, Syloti_Nagri, Syriac, Tagalog, Tagbanwa, Tai_Le, Tai_Tham, Tai_Viet, Takri, Tamil, Telugu, Thaana, Thai, Tibetan, Tifinagh, Ugaritic, Vai, Yi.

This example checks if the string consists only of Greek characters:

```sql
SELECT 'ΣΦΩ' RLIKE '^\\p{Greek}+$';
-> 1
```

### Extended Unicode Grapheme Sequence

The `\X` escape sequence matches a character sequence that makes an "extended grapheme cluster", i.e. a composite character that consists of multiple Unicode code points.

One of the examples of a composite character can be a letter followed by non-spacing accent marks. This example demonstrates that `U+0045 LATIN CAPITAL LETTER E` followed by `U+0302 COMBINING CIRCUMFLEX ACCENT` followed by `U+0323 COMBINING DOT BELOW` together form an extended grapheme cluster:

```sql
SELECT _ucs2 0x004503020323 RLIKE '^\\X$';
-> 1
```

See the [PCRE documentation](http://www.pcre.org) for the other types of extended grapheme clusters.

### Simple Assertions

An assertion specifies a certain condition that must match at a particular point, but without consuming characters from the subject string. In addition to the standard POSIX simple assertions `^` (that matches at the beginning of a line) and `$` (that matches at the end of a line), PCRE supports a number of other assertions:

<table><tbody><tr><th>Assertion</th><th>Description</th></tr>
<tr><td>\b</td><td>matches at a word boundary</td></tr>
<tr><td>\B</td><td>matches when not at a word boundary</td></tr>
<tr><td>\A</td><td>matches at the start of the subject</td></tr>
<tr><td>\Z</td><td>matches at the end of the subject, also matches before a newline at the end of the subject</td></tr>
<tr><td>\z</td><td>matches only at the end of the subject</td></tr>
<tr><td>\G</td><td>matches at the first matching position in the subject</td></tr>
</tbody></table>

This example cuts a word that consists only of 3 characters from a string:

```sql
SELECT REGEXP_SUBSTR('---abcd---xyz---', '\\b\\w{3}\\b');
-> xyz
```

Notice that the two `\b` assertions checked the word boundaries but did not get into the matching pattern.

The `\b` assertions work well in the beginning and the end of the subject string:

```sql
SELECT REGEXP_SUBSTR('xyz', '\\b\\w{3}\\b');
-> xyz
```

By default, the `^` and `$` assertions have the same meaning with `\A`, `\Z`, and `\z`. However, the meanings of `^` and `$` can change in multiline mode (see below). By contrast, the meanings of `\A`, `\Z`, and `\z` are always the same; they are independent of the multiline mode.

### Option Setting

A number of options that control the default match behavior can be changed within the pattern by a sequence of option letters enclosed between `(?` and `)`.

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td>(?i)</td><td>case insensitive match</td></tr>
<tr><td>(?m)</td><td>multiline mode</td></tr>
<tr><td>(?s)</td><td>dotall mode (dot matches newline characters)</td></tr>
<tr><td>(?x)</td><td>extended (ignore white space)</td></tr>
<tr><td>(?U)</td><td>ungreedy (lazy) match</td></tr>
<tr><td>(?J)</td><td>allow named subpatterns with duplicate names</td></tr>
<tr><td>(?X)</td><td>extra PCRE functionality (e.g. force error on unknown escaped character)</td></tr>
<tr><td>(?-...)</td><td>unset option(s)</td></tr>
</tbody></table>

For example, `(?im)` sets case insensitive multiline matching.

A hyphen followed by the option letters unset the options. For example, `(?-im)` means case sensitive single line match.

A combined setting and unsetting is also possible, e.g. `(?im-sx)`.

If an option is set outside of subpattern parentheses, the option applies to the remainder of the pattern that follows the option. If an option is set inside a subpattern, it applies to the part of this subpattern that follows the option.

In this example the pattern `(?i)m((?-i)aria)db` matches the words `MariaDB`, `Mariadb`, `mariadb`, but not `MARIADB`:

```sql
SELECT 'MariaDB' RLIKE '(?i)m((?-i)aria)db';
-> 1

SELECT 'mariaDB' RLIKE '(?i)m((?-i)aria)db';
-> 1

SELECT 'Mariadb' RLIKE '(?i)m((?-i)aria)db';
-> 1

SELECT 'MARIADB' RLIKE '(?i)m((?-i)aria)db';
-> 0
```

This example demonstrates that the `(?x)` option makes the regexp engine ignore all white spaces in the pattern (other than in a class).

```sql
SELECT 'ab' RLIKE '(?x)a b';
-> 1
```

Note, putting spaces into a pattern in combination with the (?x) option can be useful to split different logical parts of a complex pattern, to make it more readable.

### Multiline Matching

Multiline matching changes the meaning of `^` and `$` from "the beginning of the subject string" and "the end of the subject string" to "the beginning of any line in the subject string" and "the end of any line in the subject string" respectively.

This example checks if the subject string contains two consequent lines that fully consist of digits:

```sql
SELECT 'abc\n123\n456\nxyz\n' RLIKE '(?m)^\\d+\\R\\d+$';
-> 1
```

Notice the `(?m)` option in the beginning of the pattern, which switches to the multiline matching mode.

### Newline Conventions

PCRE supports five line break conventions:

- `CR (\r)` - a single carriage return character
- `LF (\n)` - a single linefeed character
- `CRLF (\r\n)` - a carriage return followed by a linefeed
- any of the previous three
- any Unicode newline sequence

By default, the newline convention is set to any Unicode newline sequence, which includes:

<table><tbody><tr><th>Sequence</th><th>Description</th></tr>
<tr><td>LF</td><td>(U+000A, carriage return)</td></tr>
<tr><td>CR</td><td>(U+000D, carriage return)</td></tr>
<tr><td>CRLF</td><td>(a carriage return followed by a linefeed)</td></tr>
<tr><td>VT</td><td>(U+000B, vertical tab)</td></tr>
<tr><td>FF</td><td>(U+000C, form feed)</td></tr>
<tr><td>NEL</td><td>(U+0085, next line)</td></tr>
<tr><td>LS</td><td>(U+2028, line separator)</td></tr>
<tr><td>PS</td><td>(U+2029, paragraph separator)</td></tr>
</tbody></table>

The newline convention can be set by starting a pattern with one of the following sequences:

<table><tbody><tr><th>Sequence</th><th>Description</th></tr>
<tr><td>(*CR)</td><td>carriage return</td></tr>
<tr><td>(*LF)</td><td>linefeed</td></tr>
<tr><td>(*CRLF)</td><td>carriage return followed by linefeed</td></tr>
<tr><td>(*ANYCRLF)</td><td>any of the previous three</td></tr>
<tr><td>(*ANY)</td><td>all Unicode newline sequences</td></tr>
</tbody></table>

The newline conversion affects the `^` and `$` assertions, the interpretation of the dot metacharacter, and the behavior of `\N`.

Note, the new line convention does not affect the meaning of `\R`.

This example demonstrates that the dot metacharacter matches `\n`, because it is not a newline sequence anymore:

```sql
SELECT 'a\nb' RLIKE '(*CR)a.b';
-> 1
```

### Newline Sequences

By default, the escape sequence `\R` matches any Unicode newline sequences.

The meaning of `\R` can be set by starting a pattern with one of the following sequences:

<table><tbody><tr><th>Sequence</th><th>Description</th></tr>
<tr><td>(*BSR_ANYCRLF)</td><td>any of CR, LF or CRLF</td></tr>
<tr><td>(*BSR_UNICODE)</td><td>any Unicode newline sequence</td></tr>
</tbody></table>

### Comments

It's possible to include comments inside a pattern. Comments do not participate in the pattern matching. Comments start at the `(?`# sequence and continue up to the next closing parenthesis:

```sql
SELECT 'ab12' RLIKE 'ab(?#expect digits)12';
-> 1
```

### Quoting

POSIX uses the backslash to remove a special meaning from a character. PCRE introduces a syntax to remove special meaning from a sequence of characters. The characters inside `\Q` ... `\E` are treated literally, without their special meaning.

This example checks if the string matches a dollar sign followed by a parenthesized name (a variable reference in some languages):

```sql
SELECT '$(abc)' RLIKE '^\\Q$(\\E\\w+\\Q)\\E$';
-> 1
```

Note that the leftmost dollar sign and the parentheses are used literally, while the rightmost dollar sign is still used to match the end of the string.

### Resetting the Match Start

The escape sequence `\K` causes any previously matched characters to be excluded from the final matched sequence. For example, the pattern: `(foo)\Kbar` matches `foobar`, but reports that it has matched `bar`. This feature is similar to a look-behind assertion. However, in this case, the part of the subject before the real match does not have to be of fixed length:

```sql
SELECT REGEXP_SUBSTR('aaa123', '[a-z]*\\K[0-9]*');
-> 123
```

### Non-Capturing Groups

The question mark and the colon after the opening parenthesis create a non-capturing group: `(?:...)`.

This example removes an optional article from a word, for example for better sorting of the results.

```sql
SELECT REGEXP_REPLACE('The King','(?:the|an|a)[^a-z]([a-z]+)','\\1');
-> King
```

Note that the articles are listed inside the left parentheses using the alternation operator `|` but they do not produce a captured subpattern, so the word followed by the article is referenced by `'<br>1'` in the third argument to the function. Using non-capturing groups can be useful to save numbers on the sup-patterns that won't be used in the third argument of [REGEXP_REPLACE()](/built-in-functions/string-functions/regular-expressions-functions/regexp_replace), as well as for performance purposes.

### Non-Greedy Quantifiers

By default, the repetition quantifiers `?`, `*`, `+` and `{n,m}` are "greedy", that is, they try to match as much as possible. Adding a question mark after a repetition quantifier makes it "non-greedy", so the pattern matches the minimum number of times possible.

This example cuts C comments from a line:

```sql
SELECT REGEXP_REPLACE('/* Comment1 */ i+= 1; /* Comment2 */', '/[*].*?[*]/','');
->  i+= 1;
```

The pattern without the non-greedy flag to the quantifier `/[*].*[*]/` would match the entire string between the leftmost `/*` and the rightmost `*/`.

### Atomic Groups

A sequence inside `(?&gt;`...`)` makes an atomic group. Backtracking inside an atomic group is prevented once it has matched; however, backtracking past to the previous items works normally.

Consider the pattern `\d+foo` applied to the subject string `123bar`. Once the engine scans `123` and fails on the letter `b`, it would normally backtrack to `2` and try to match again, then fail and backtrack to `1` and try to match and fail again, and finally fail the entire pattern. In case of an atomic group `(?&gt;\d+)foo` with the same subject string `123bar`, the engine gives up immediately after the first failure to match `foo`. An atomic group with a quantifier can match all or nothing.

Atomic groups produce faster false results (i.e. in case when a long subject string does not match the pattern), because the regexp engine saves performance on backtracking. However, don't hurry to put everything into atomic groups. This example demonstrates the difference between atomic and non-atomic match:

```sql
SELECT 'abcc' RLIKE 'a(?>bc|b)c' AS atomic1;
-> 1

SELECT 'abc' RLIKE 'a(?>bc|b)c' AS atomic2;
-> 0
 
SELECT 'abcc' RLIKE 'a(bc|b)c' AS non_atomic1;
-> 1

SELECT 'abc' RLIKE 'a(bc|b)c' AS non_atomic2;
-> 1
```

The non-atomic pattern matches both `abbc` and `abc`, while the atomic pattern matches `abbc` only.

The atomic group `(?&gt;bc|b)` in the above example can be "translated" as "if there is `bc`, then don't try to match as `b`". So `b` can match only if `bc` is not found.

Atomic groups are not capturing. To make an atomic group capturing, put it into parentheses:

```sql
SELECT REGEXP_REPLACE('abcc','a((?>bc|b))c','\\1');
-> bc
```

### Possessive quantifiers

An atomic group which ends with a quantifier can be rewritten using a so called "possessive quantifier" syntax by putting an additional `+` sign following the quantifier.

The pattern `(?&gt;\d+)foo` from the previous section's example can be rewritten as `\d++foo`.

### Absolute and Relative Numeric Backreferences

Backreferences match the same text as previously matched by a capturing group. Backreferences can be written using:

- a backslash followed by a digit
- the `\g` escape sequence followed by a positive or negative number
- the `\g` escape sequence followed by a positive or negative number enclosed in braces

The following backreferences are identical and refer to the first capturing group:

- `\1`
- `\g1`
- `\g{1}`

This example demonstrates a pattern that matches "sense and sensibility" and "response and responsibility", but not "sense and responsibility":

```sql
SELECT 'sense and sensibility' RLIKE '(sens|respons)e and \\1ibility';
-> 1
```

This example removes doubled words that can unintentionally creep in when you edit a text in a text editor:

```sql
SELECT REGEXP_REPLACE('using using the the regexp regexp',
 '\\b(\\w+)\\s+\\1\\b','\\1');
-> using the regexp
```

Note that all double words were removed, in the beginning, in the middle and in the end of the subject string.

A negative number in a `\g` sequence means a relative reference. Relative references can be helpful in long patterns, and also in patterns that are created by joining fragments together that contain references within themselves. The sequence `\g{-1}` is a reference to the most recently started capturing subpattern before `\g`.

In this example `\g{-1}` is equivalent to `\2`:

```sql
SELECT 'abc123def123' RLIKE '(abc(123)def)\\g{-1}';     
-> 1

SELECT 'abc123def123' RLIKE '(abc(123)def)\\2';
-> 1
```

### Named Subpatterns and Backreferences

Using numeric backreferences for capturing groups can be hard to track in a complicated regular expression. Also, the numbers can change if an expression is modified. To overcome these difficulties, PCRE supports named subpatterns.

A subpattern can be named in one of three ways: `(?&lt;name&gt;`...`)` or `(?'name'`...`)` as in Perl, or `(?P&lt;name&gt;`...`)` as in Python. References to capturing subpatterns from other parts of the pattern, can be made by name as well as by number.

Backreferences to a named subpattern can be written using the .NET syntax `\k{name}`, the Perl syntax `\k&lt;name&gt;` or `\k'name'` or `\g{name}`, or the Python syntax `(?P=name)`.

This example tests if the string is a correct HTML tag:

```sql
SELECT '<a href="../">Up</a>' RLIKE '<(?<tag>[a-z][a-z0-9]*)[^>]*>[^<]*</(?P=tag)>';
-> 1 
```

### Positive and Negative Look-Ahead and Look-Behind Assertions

Look-ahead and look-behind assertions serve to specify the context for the searched regular expression pattern. Note that the assertions only check the context, they do not capture anything themselves!

This example finds the letter which is not followed by another letter (negative look-ahead):

```sql
SELECT REGEXP_SUBSTR('ab1','[a-z](?![a-z])');
-> b
```

This example finds the letter which is followed by a digit (positive look-ahead):

```sql
SELECT REGEXP_SUBSTR('ab1','[a-z](?=[0-9])');
-> b
```

This example finds the letter which does not follow a digit character (negative look-behind):

```sql
SELECT REGEXP_SUBSTR('1ab','(?<![0-9])[a-z]');
-> b
```

This example finds the letter which follows another letter character (positive look-behind):

```sql
SELECT REGEXP_SUBSTR('1ab','(?<=[a-z])[a-z]');
-> b
```

Note that look-behind assertions can only be of fixed length; you cannot have repetition operators or alternations with different lengths:

```sql
SELECT 'aaa' RLIKE '(?<=(a|bc))a';
ERROR 1139 (42000): Got error 'lookbehind assertion is not fixed length at offset 10' from regexp
```

### Subroutine Reference and Recursive Patterns

PCRE supports a special syntax to recourse the entire pattern or its individual subpatterns:

<table><tbody><tr><th>Syntax</th><th>Description</th></tr>
<tr><td>(?R)</td><td>Recourse the entire pattern</td></tr>
<tr><td>(?n)</td><td>call subpattern by absolute number</td></tr>
<tr><td>(?+n)</td><td>call subpattern by relative number</td></tr>
<tr><td>(?-n)</td><td>call subpattern by relative number</td></tr>
<tr><td>(?&amp;name)</td><td>call subpattern by name (Perl)</td></tr>
<tr><td>(?P&gt;name)</td><td>call subpattern by name (Python)</td></tr>
<tr><td>\g&lt;name&gt;</td><td>call subpattern by name (Oniguruma)</td></tr>
<tr><td>\g'name'</td><td>call subpattern by name (Oniguruma)</td></tr>
<tr><td>\g&lt;n&gt;</td><td>call subpattern by absolute number (Oniguruma)</td></tr>
<tr><td>\g'n'</td><td>call subpattern by absolute number (Oniguruma)</td></tr>
<tr><td>\g&lt;+n&gt;</td><td>call subpattern by relative number</td></tr>
<tr><td>\g&lt;-n&gt;</td><td>call subpattern by relative number</td></tr>
<tr><td>\g'+n'</td><td>call subpattern by relative number</td></tr>
<tr><td>\g'-n'</td><td>call subpattern by relative number</td></tr>
</tbody></table>

This example checks for a correct additive arithmetic expression consisting of numbers, unary plus and minus, binary plus and minus, and parentheses:

```sql
SELECT '1+2-3+(+(4-1)+(-2)+(+1))' RLIKE  '^(([+-]?(\\d+|[(](?1)[)]))(([+-](?1))*))$';
-> 1
```

The recursion is done using `(?1)` to call for the first parenthesized subpattern, which includes everything except the leading `^` and the trailing `$`.

The regular expression in the above example implements the following BNF grammar:

1 <code class="fixed" style="white-space:pre-wrap">&lt;expression&gt; ::= &lt;term&gt; [(&lt;sign&gt; &lt;term&gt;)...]</code>
2 <code class="fixed" style="white-space:pre-wrap">&lt;term&gt; ::= [ &lt;sign&gt; ] &lt;primary&gt;</code>
3 <code class="fixed" style="white-space:pre-wrap">&lt;primary&gt; ::= &lt;number&gt; | &lt;left paren&gt; &lt;expression&gt; &lt;right paren&gt;</code>
4 <code class="fixed" style="white-space:pre-wrap">&lt;sign&gt; ::= &lt;plus sign&gt; | &lt;minus sign&gt;</code>

### Defining Subpatterns For Use By Reference

Use the `(?(DEFINE)`...`)` syntax to define subpatterns that can be referenced from elsewhere.

This example defines a subpattern with the name `letters` that matches one or more letters, which is further reused two times:

```sql
SELECT 'abc123xyz' RLIKE '^(?(DEFINE)(?<letters>[a-z]+))(?&letters)[0-9]+(?&letters)$';
-> 1
```

The above example can also be rewritten to define the digit part as a subpattern as well:

```sql
SELECT 'abc123xyz' RLIKE
 '^(?(DEFINE)(?<letters>[a-z]+)(?<digits>[0-9]+))(?&letters)(?&digits)(?&letters)$';
-> 1
```

### Conditional Subpatterns

There are two forms of conditional subpatterns:

```sql
(?(condition)yes-pattern)
(?(condition)yes-pattern|no-pattern)
```

The `yes-pattern` is used if the condition is satisfied, otherwise the `no-pattern` (if any) is used.

#### Conditions With Subpattern References

If a condition consists of a number, it makes a condition with a subpattern reference. Such a condition is true if a capturing subpattern corresponding to the number has previously matched.

This example finds an optionally parenthesized number in a string:

```sql
SELECT REGEXP_SUBSTR('a(123)b', '([(])?[0-9]+(?(1)[)])');
-> (123)
```

The `([(])?` part makes a capturing subpattern that matches an optional opening parenthesis; the `[0-9]+` part matches a number, and the `(?(1)[)])` part matches a closing parenthesis, but only if the opening parenthesis has been previously found.

#### Other Kinds of Conditions

The other possible condition kinds are: recursion references and assertions. See the [PCRE documentation](http://www.pcre.org) for details.

### Matching Zero Bytes (0x00)

PCRE correctly works with zero bytes in the subject strings:

```sql
SELECT 'a\0b' RLIKE '^a.b$';
-> 1
```

Zero bytes, however, are not supported literally in the pattern strings and should be escaped using the `\xhh` or `\x{hh}` syntax:

```sql
SELECT 'a\0b' RLIKE '^a\\x{00}b$';
-> 1
```

### Other PCRE Features

PCRE provides other extended features that were not covered in this document,
such as duplicate subpattern numbers, backtracking control, breaking utf-8
sequences into individual bytes, setting the match limit, setting the recursion
limit, optimization control, recursion conditions, assertion conditions and
more types of extended grapheme clusters. Please refer to the
[PCRE documentation](http://www.pcre.org) for details.

Enhanced regex was implemented as a GSoC 2013 project by  Sudheera Palihakkara.

### default_regex_flags Examples

##### MariaDB starting with [10.0.11](/kb/en/mariadb-10011-release-notes/)

The [default_regex_flags](/kb/en/server-system-variables/#default_regex_flags) variable was introduced in <strong>MariaDB</strong> 10.0.11

The [default_regex_flags](/kb/en/server-system-variables/#default_regex_flags)  variable was introduced to address the remaining incompatibilities between PCRE and the old regex library. Here are some examples of its usage:

The default behaviour (multiline match is off)

```sql
SELECT 'a\nb\nc' RLIKE '^b$';
+---------------------------+
| '(?m)a\nb\nc' RLIKE '^b$' |
+---------------------------+
|                         0 |
+---------------------------+
```

Enabling the multiline option using the PCRE option syntax:

```sql
SELECT 'a\nb\nc' RLIKE '(?m)^b$';
+---------------------------+
| 'a\nb\nc' RLIKE '(?m)^b$' |
+---------------------------+
|                         1 |
+---------------------------+
```

Enabling the miltiline option using default_regex_flags

```sql
SET default_regex_flags='MULTILINE';
SELECT 'a\nb\nc' RLIKE '^b$';
+-----------------------+
| 'a\nb\nc' RLIKE '^b$' |
+-----------------------+
|                     1 |
+-----------------------+ 
```

### See Also

- [MariaDB upgrades to PCRE-8.34](https://blog.mariadb.org/mariadb-upgrades-to-pcre-8-34/)