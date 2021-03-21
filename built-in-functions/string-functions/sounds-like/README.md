# SOUNDS LIKE

## Syntax

```sql
expr1 SOUNDS LIKE expr2
```

## Description

This is the same as [SOUNDEX](/built-in-functions/string-functions/soundex)(expr1) = SOUNDEX(expr2).

## Example

```sql
SELECT givenname, surname FROM users WHERE givenname SOUNDS LIKE "robert";
+-----------+---------+
| givenname | surname |
+-----------+---------+
| Roberto   | Castro  |
+-----------+---------+
```