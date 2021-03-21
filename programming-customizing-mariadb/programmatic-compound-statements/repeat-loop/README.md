# REPEAT LOOP

## Syntax

```sql
[begin_label:] REPEAT
    statement_list
UNTIL search_condition
END REPEAT [end_label]
```

The statement list within a `REPEAT` statement is repeated until the
search_condition is true. Thus, a `REPEAT` always enters the loop at
least once. statement_list consists of one or more statements, each
terminated by a semicolon (i.e., `;`) statement delimiter.

A `REPEAT` statement can be [labeled](/programming-customizing-mariadb/programmatic-compound-statements/labels). end_label cannot be given unless
begin_label also is present. If both are present, they must be the
same.

See [Delimiters](/clients-utilities/mysql-client/delimiters) in the [mysql](/clients-utilities/mysql-client) client for more on client delimiter usage.

```sql
DELIMITER //

CREATE PROCEDURE dorepeat(p1 INT)
  BEGIN
    SET @x = 0;
    REPEAT SET @x = @x + 1; UNTIL @x > p1 END REPEAT;
  END
//

CALL dorepeat(1000)//

SELECT @x//
+------+
| @x   |
+------+
| 1001 |
+------+
```