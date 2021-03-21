# SHOW CREATE FUNCTION

## Syntax

```sql
SHOW CREATE FUNCTION func_name
```

## Description

This statement is similar to 
<code class="highlight fixed" style="white-space:pre-wrap">[SHOW CREATE PROCEDURE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-procedure/)</code> but for
[stored functions](/programming-customizing-mariadb/stored-routines/stored-functions/).

The output of this statement is unreliably affected by the <a undefined>sql_quote_show_create</a> server system variable - see [http://bugs.mysql.com/bug.php?id=12719](http://bugs.mysql.com/bug.php?id=12719)

## Example

```sql
MariaDB [test]> SHOW CREATE FUNCTION VatCents\G
*************************** 1. row ***************************
            Function: VatCents
            sql_mode: 
     Create Function: CREATE DEFINER=`root`@`localhost` FUNCTION `VatCents`(price DECIMAL(10,2)) RETURNS int(11)
    DETERMINISTIC
BEGIN
 DECLARE x INT;
 SET x = price * 114;
 RETURN x;
END
character_set_client: utf8
collation_connection: utf8_general_ci
  Database Collation: latin1_swedish_ci
```

## See also:

- [Stored Functions](/programming-customizing-mariadb/stored-routines/stored-functions/)
- <code class="highlight fixed" style="white-space:pre-wrap">[CREATE FUNCTION](/sql-statements-structure/sql-statements/data-definition/create/create-function/)</code>