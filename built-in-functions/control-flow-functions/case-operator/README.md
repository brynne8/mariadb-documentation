# CASE OPERATOR

## Syntax

```sql
CASE value WHEN [compare_value] THEN result [WHEN [compare_value] THEN
result ...] [ELSE result] END

CASE WHEN [condition] THEN result [WHEN [condition] THEN result ...]
[ELSE result] END
```

## Description

The first version returns the result where value=compare_value. The
second version returns the result for the first condition that is
true.  If there was no matching result value, the result after ELSE is
returned, or NULL if there is no ELSE part.

There is also a [CASE statement](/programming-customizing-mariadb/programmatic-compound-statements/case-statement), which differs from the CASE operator described here.

## Examples

```sql
SELECT CASE 1 WHEN 1 THEN 'one' WHEN 2 THEN 'two' ELSE 'more' END;
+------------------------------------------------------------+
| CASE 1 WHEN 1 THEN 'one' WHEN 2 THEN 'two' ELSE 'more' END |
+------------------------------------------------------------+
| one                                                        |
+------------------------------------------------------------+

SELECT CASE WHEN 1>0 THEN 'true' ELSE 'false' END;
+--------------------------------------------+
| CASE WHEN 1>0 THEN 'true' ELSE 'false' END |
+--------------------------------------------+
| true                                       |
+--------------------------------------------+


SELECT CASE BINARY 'B' WHEN 'a' THEN 1 WHEN 'b' THEN 2 END;
+-----------------------------------------------------+
| CASE BINARY 'B' WHEN 'a' THEN 1 WHEN 'b' THEN 2 END |
+-----------------------------------------------------+
|                                                NULL |
+-----------------------------------------------------+
```