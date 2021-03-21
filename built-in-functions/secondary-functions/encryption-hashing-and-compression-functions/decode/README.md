# DECODE

## Syntax

```sql
DECODE(crypt_str,pass_str)
```

In [Oracle mode](/kb/en/sql_modeoracle/) from [MariaDB 10.3.2](/kb/en/mariadb-1032-release-notes/):

```sql
DECODE(expr, search_expr, result_expr [, search_expr2, result_expr2 ...] [default_expr])
```

In all modes from [MariaDB 10.3.2](/kb/en/mariadb-1032-release-notes/):

```sql
DECODE_ORACLE(expr, search_expr, result_expr [, search_expr2, result_expr2 ...] [default_expr])
```

## Description

In the default mode, `DECODE` decrypts the encrypted string <em>crypt_str</em> using <em>pass_str</em> as the
password. <em>crypt_str</em> should be a string returned from [ENCODE()](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/encode/). The resulting string will be the original string only if <em>pass_str</em> is the same.

In [Oracle mode](/kb/en/sql_modeoracle/) from [MariaDB 10.3.2](/kb/en/mariadb-1032-release-notes/), `DECODE` compares <em>expr</em> to the search expressions, in order. If it finds a match, the corresponding result expression is returned. If no matches are found, the default expression is returned, or NULL if no default is provided.

NULLs are treated as equivalent.

`DECODE_ORACLE` is a synonym for the Oracle-mode version of the function, and is available in all modes.

## Examples

From [MariaDB 10.3.2](/kb/en/mariadb-1032-release-notes/):

```sql
SELECT DECODE_ORACLE(2+1,3*1,'found1',3*2,'found2','default');
+--------------------------------------------------------+
| DECODE_ORACLE(2+1,3*1,'found1',3*2,'found2','default') |
+--------------------------------------------------------+
| found1                                                 |
+--------------------------------------------------------+

SELECT DECODE_ORACLE(2+4,3*1,'found1',3*2,'found2','default');
+--------------------------------------------------------+
| DECODE_ORACLE(2+4,3*1,'found1',3*2,'found2','default') |
+--------------------------------------------------------+
| found2                                                 |
+--------------------------------------------------------+

SELECT DECODE_ORACLE(2+2,3*1,'found1',3*2,'found2','default');
+--------------------------------------------------------+
| DECODE_ORACLE(2+2,3*1,'found1',3*2,'found2','default') |
+--------------------------------------------------------+
| default                                                |
+--------------------------------------------------------+
```

Nulls are treated as equivalent:

```sql
SELECT DECODE_ORACLE(NULL,NULL,'Nulls are equivalent','Nulls are not equivalent');
+----------------------------------------------------------------------------+
| DECODE_ORACLE(NULL,NULL,'Nulls are equivalent','Nulls are not equivalent') |
+----------------------------------------------------------------------------+
| Nulls are equivalent                                                       |
+----------------------------------------------------------------------------+
```