# MAKE_SET

## Syntax

```sql
MAKE_SET(bits,str1,str2,...)
```

## Description

Returns a set value (a string containing substrings separated by ","
characters) consisting of the strings that have the corresponding bit
in bits set. <em>`str1`</em> corresponds to bit 0, <em>`str2`</em> to bit 1, and so on. NULL
values in <em>`str1`</em>, <em>`str2`</em>, ... are not appended to the result.

## Examples

```sql
SELECT MAKE_SET(1,'a','b','c');
+-------------------------+
| MAKE_SET(1,'a','b','c') |
+-------------------------+
| a                       |
+-------------------------+

SELECT MAKE_SET(1 | 4,'hello','nice','world');
+----------------------------------------+
| MAKE_SET(1 | 4,'hello','nice','world') |
+----------------------------------------+
| hello,world                            |
+----------------------------------------+

SELECT MAKE_SET(1 | 4,'hello','nice',NULL,'world');
+---------------------------------------------+
| MAKE_SET(1 | 4,'hello','nice',NULL,'world') |
+---------------------------------------------+
| hello                                       |
+---------------------------------------------+

SELECT QUOTE(MAKE_SET(0,'a','b','c'));
+--------------------------------+
| QUOTE(MAKE_SET(0,'a','b','c')) |
+--------------------------------+
| ''                             |
+--------------------------------+
```