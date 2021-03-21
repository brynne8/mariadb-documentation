# WHILE

## Syntax

```sql
[begin_label:] WHILE search_condition DO
    statement_list
END WHILE [end_label]
```

## Description

The statement list within a `WHILE` statement is repeated as long as the
`search_condition` is true. statement_list consists of one or more
statements. If the loop must be executed at least once, [REPEAT ... LOOP](/programming-customizing-mariadb/programmatic-compound-statements/repeat-loop/) can be used instead.

A `WHILE` statement can be [labeled](/programming-customizing-mariadb/programmatic-compound-statements/labels/). end_label cannot be given unless
begin_label also is present. If both are present, they must be the
same.

## Examples

```sql
CREATE PROCEDURE dowhile()
BEGIN
  DECLARE v1 INT DEFAULT 5;

  WHILE v1 > 0 DO
    ...
    SET v1 = v1 - 1;
  END WHILE;
END
```