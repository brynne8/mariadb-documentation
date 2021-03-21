# MID

## Syntax

```sql
MID(str,pos,len)
```

## Description

MID(str,pos,len) is a synonym for [SUBSTRING(str,pos,len)](/built-in-functions/string-functions/substring).

## Examples

```sql
SELECT MID('abcd',4,1);
+-----------------+
| MID('abcd',4,1) |
+-----------------+
| d               |
+-----------------+

SELECT MID('abcd',2,2);
+-----------------+
| MID('abcd',2,2) |
+-----------------+
| bc              |
+-----------------+
```

A negative starting position:

```sql
SELECT MID('abcd',-2,4);
+------------------+
| MID('abcd',-2,4) |
+------------------+
| cd               |
+------------------+
```