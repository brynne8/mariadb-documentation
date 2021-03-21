# SUBSTRING

## Syntax

```sql
SUBSTRING(str,pos), 
SUBSTRING(str FROM pos), 
SUBSTRING(str,pos,len),
SUBSTRING(str FROM pos FOR len)

SUBSTR(str,pos), 
SUBSTR(str FROM pos), 
SUBSTR(str,pos,len),
SUBSTR(str FROM pos FOR len)
```

## Description

The forms without a <em>`len`</em> argument return a substring from string <em>`str`</em> starting at position <em>`pos`</em>.

The forms with a <em>`len`</em> argument return a substring <em>`len`</em> characters long from string <em>`str`</em>, starting at position <em>`pos`</em>.

The forms that use <em>`FROM`</em> are standard SQL syntax.

It is also possible to use a negative value for <em>`pos`</em>. In this case, the beginning of the substring is <em>`pos`</em> characters from the end of the string, rather than the beginning. A negative value may be used for <em>`pos`</em> in any of the forms of this function.

By default, the position of the first character in the string from which the substring is to be extracted is reckoned as 1. For [Oracle-compatibility](/kb/en/sql_modeoracle-from-mariadb-103/), from [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/), when sql_mode is set to 'oracle', position zero is treated as position 1 (although the first character is still reckoned as 1).

If any argument is `NULL`, returns `NULL`.

## Examples

```sql
SELECT SUBSTRING('Knowledgebase',5);
+------------------------------+
| SUBSTRING('Knowledgebase',5) |
+------------------------------+
| ledgebase                    |
+------------------------------+

SELECT SUBSTRING('MariaDB' FROM 6);
+-----------------------------+
| SUBSTRING('MariaDB' FROM 6) |
+-----------------------------+
| DB                          |
+-----------------------------+

SELECT SUBSTRING('Knowledgebase',3,7);
+--------------------------------+
| SUBSTRING('Knowledgebase',3,7) |
+--------------------------------+
| owledge                        |
+--------------------------------+

SELECT SUBSTRING('Knowledgebase', -4);
+--------------------------------+
| SUBSTRING('Knowledgebase', -4) |
+--------------------------------+
| base                           |
+--------------------------------+

SELECT SUBSTRING('Knowledgebase', -8, 4);
+-----------------------------------+
| SUBSTRING('Knowledgebase', -8, 4) |
+-----------------------------------+
| edge                              |
+-----------------------------------+

SELECT SUBSTRING('Knowledgebase' FROM -8 FOR 4);
+------------------------------------------+
| SUBSTRING('Knowledgebase' FROM -8 FOR 4) |
+------------------------------------------+
| edge                                     |
+------------------------------------------+
```

[Oracle mode from MariaDB 10.3.3](/kb/en/sql_modeoracle-from-mariadb-103/):

```sql
SELECT SUBSTR('abc',0,3);
+-------------------+
| SUBSTR('abc',0,3) |
+-------------------+
|                   |
+-------------------+

SELECT SUBSTR('abc',1,2);
+-------------------+
| SUBSTR('abc',1,2) |
+-------------------+
| ab                |
+-------------------+

SET sql_mode='oracle';

SELECT SUBSTR('abc',0,3);
+-------------------+
| SUBSTR('abc',0,3) |
+-------------------+
| abc               |
+-------------------+

SELECT SUBSTR('abc',1,2);
+-------------------+
| SUBSTR('abc',1,2) |
+-------------------+
| ab                |
+-------------------+
```

## See Also

- [INSTR()](/built-in-functions/string-functions/instr) - Returns the position of a string within a string
- [LOCATE()](/built-in-functions/string-functions/locate) - Returns the position of a string within a string
- [SUBSTRING_INDEX()](/built-in-functions/string-functions/substring_index) - Returns a string based on substring