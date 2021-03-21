# INTERVAL

## Syntax

```sql
INTERVAL(N,N1,N2,N3,...)
```

## Description

Returns the index of the last argument that is less than the first argument or is NULL.

Returns 0 if N &lt; N1, 1 if N &lt; N2, 2 if N &lt; N3 and so on or -1 if N is NULL. All
arguments are treated as integers. It is required that N1 &lt; N2 &lt; N3 &lt;
... &lt; Nn for this function to work correctly. This is because a fast binary
search is used.

## Examples

```sql
SELECT INTERVAL(23, 1, 15, 17, 30, 44, 200);
+--------------------------------------+
| INTERVAL(23, 1, 15, 17, 30, 44, 200) |
+--------------------------------------+
|                                    3 |
+--------------------------------------+

SELECT INTERVAL(10, 1, 10, 100, 1000);
+--------------------------------+
| INTERVAL(10, 1, 10, 100, 1000) |
+--------------------------------+
|                              2 |
+--------------------------------+

SELECT INTERVAL(22, 23, 30, 44, 200);
+-------------------------------+
| INTERVAL(22, 23, 30, 44, 200) |
+-------------------------------+
|                             0 |
+-------------------------------+

SELECT INTERVAL(10, 2, NULL);
+-----------------------+
| INTERVAL(10, 2, NULL) |
+-----------------------+
|                     2 |
+-----------------------+
```