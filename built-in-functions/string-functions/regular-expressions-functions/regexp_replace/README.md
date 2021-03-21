# REGEXP_REPLACE

##### MariaDB starting with [10.0.5](/kb/en/mariadb-1005-release-notes/)

REGEXP_REPLACE was introduced in [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/).

## Syntax

```sql
REGEXP_REPLACE(subject, pattern, replace)
```

## Description

`REGEXP_REPLACE` returns the string `subject` with all occurrences of the regular expression `pattern` replaced by the string `replace`. If no occurrences are found, then `subject` is returned as is.

The replace string can have backreferences to the subexpressions in the form \N, where N is a number from 1 to 9.

The function follows the case sensitivity rules of the effective [collation](/kb/en/data-types-character-sets-and-collations/). Matching is performed case insensitively for case insensitive collations, and case sensitively for case sensitive collations and for binary data.

The collation case sensitivity can be overwritten using the (?i) and (?-i) PCRE flags.

[MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/) switched to the [PCRE regular expression](/kb/en/pcre-regular-expressions/) library for enhanced regular expression performance, and `REGEXP_REPLACE` was introduced as part of this enhancement.

[MariaDB 10.0.11](/kb/en/mariadb-10011-release-notes/) introduced the [default_regex_flags](/kb/en/server-system-variables/#default_regex_flags) variable to address the remaining compatibilities between PCRE and the old regex library.

## Examples

```sql
SELECT REGEXP_REPLACE('ab12cd','[0-9]','') AS remove_digits;
-> abcd

SELECT REGEXP_REPLACE('<html><head><title>title</title><body>body</body></htm>', '<.+?>',' ')
AS strip_html;
-> title  body
```

Backreferences to the subexpressions in the form `\N`, where N is a number from 1 to 9:

```sql
SELECT REGEXP_REPLACE('James Bond','^(.*) (.*)$','\\2, \\1') AS reorder_name;
-> Bond, James
```

Case insensitive and case sensitive matches:

```sql
SELECT REGEXP_REPLACE('ABC','b','-') AS case_insensitive;
-> A-C

SELECT REGEXP_REPLACE('ABC' COLLATE utf8_bin,'b','-') AS case_sensitive;
-> ABC

SELECT REGEXP_REPLACE(BINARY 'ABC','b','-') AS binary_data;
-> ABC
```

Overwriting the collation case sensitivity using the (?i) and (?-i) PCRE flags.

```sql
SELECT REGEXP_REPLACE('ABC','(?-i)b','-') AS force_case_sensitive;
-> ABC

SELECT REGEXP_REPLACE(BINARY 'ABC','(?i)b','-') AS force_case_insensitive;
-> A-C
```