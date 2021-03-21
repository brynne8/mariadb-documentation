# REGEXP_INSTR

##### MariaDB starting with [10.0.5](/kb/en/mariadb-1005-release-notes/)

REGEXP_INSTR was introduced in [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/).

## Syntax

```sql
REGEXP_INSTR(subject, pattern)
```

Returns the position of the first occurrence of the regular expression `pattern` in the string `subject`, or 0 if pattern was not found.

The positions start with 1 and are measured in characters (i.e. not in bytes), which is important for multi-byte character sets. You can cast a multi-byte character set to [BINARY](/columns-storage-engines-and-plugins/data-types/string-data-types/binary) to get offsets in bytes.

The function follows the case sensitivity rules of the effective [collation](/kb/en/data-types-character-sets-and-collations/). Matching is performed case insensitively for case insensitive collations, and case sensitively for case sensitive collations and for binary data.

The collation case sensitivity can be overwritten using the (?i) and (?-i) PCRE flags.

[MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/) switched to the [PCRE regular expression](/kb/en/pcre-regular-expressions/) library for enhanced regular expression performance, and REGEXP_INSTR was introduced as part of this enhancement.

## Examples

```sql
SELECT REGEXP_INSTR('abc','b');
-> 2

SELECT REGEXP_INSTR('abc','x');
-> 0

SELECT REGEXP_INSTR('BJÖRN','N');
-> 5
```

Casting a multi-byte character set as BINARY to get offsets in bytes:

```sql
SELECT REGEXP_INSTR(BINARY 'BJÖRN','N') AS cast_utf8_to_binary;
-> 6
```

Case sensitivity:

```sql
SELECT REGEXP_INSTR('ABC','b');
-> 2

SELECT REGEXP_INSTR('ABC' COLLATE utf8_bin,'b');
-> 0

SELECT REGEXP_INSTR(BINARY'ABC','b');
-> 0

SELECT REGEXP_INSTR('ABC','(?-i)b');
-> 0

SELECT REGEXP_INSTR('ABC' COLLATE utf8_bin,'(?i)b');
-> 2
```