# ITERATE

## Syntax

```sql
ITERATE label
```

`ITERATE` can appear only within [LOOP](/programming-customizing-mariadb/programmatic-compound-statements/loop/), [REPEAT](/programming-customizing-mariadb/programmatic-compound-statements/repeat-loop/), and [WHILE](/programming-customizing-mariadb/programmatic-compound-statements/while/) statements.
`ITERATE` means "do the loop again", and uses the statement's [label](/programming-customizing-mariadb/programmatic-compound-statements/labels/) to determine which statements to repeat. The label must be in the same stored program, not in a caller procedure.

If you try to use `ITERATE` with a non-existing label, or if the label is associated to a construct which is not a loop, the following error will be produced:

```sql
ERROR 1308 (42000): ITERATE with no matching label: <label_name>
```

Below is an example of how `ITERATE` might be used:

```sql
CREATE PROCEDURE doiterate(p1 INT)
BEGIN
  label1: LOOP
    SET p1 = p1 + 1;
    IF p1 < 10 THEN ITERATE label1; END IF;
    LEAVE label1;
  END LOOP label1;
  SET @x = p1;
END
```

## See Also

- [LEAVE](/programming-customizing-mariadb/programmatic-compound-statements/leave/) - Exits a loop (or any labeled code block)