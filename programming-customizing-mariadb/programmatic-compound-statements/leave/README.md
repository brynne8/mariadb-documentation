# LEAVE

## Syntax

```sql
LEAVE label
```

This statement is used to exit the flow control construct that has the
given [label](/programming-customizing-mariadb/programmatic-compound-statements/labels). The label must be in the same stored program, not in a caller procedure. `LEAVE` can be used within [BEGIN ... END](/programming-customizing-mariadb/programmatic-compound-statements/begin-end) or loop constructs
([LOOP](/programming-customizing-mariadb/programmatic-compound-statements/loop), [REPEAT](/programming-customizing-mariadb/programmatic-compound-statements/repeat-loop), [WHILE](/programming-customizing-mariadb/programmatic-compound-statements/while)). In [Stored Procedures](/programming-customizing-mariadb/stored-routines/stored-procedures), [Triggers](/programming-customizing-mariadb/triggers-events/triggers) and [Events](/programming-customizing-mariadb/triggers-events/event-scheduler/events), LEAVE can refer to the outmost BEGIN ... END construct; in that case, the program exits the procedure. In [Stored Functions](/programming-customizing-mariadb/stored-routines/stored-functions), [RETURN](/programming-customizing-mariadb/programmatic-compound-statements/return) can be used instead.

Note that LEAVE cannot be used to exit a [DECLARE HANDLER](/programming-customizing-mariadb/programmatic-compound-statements/declare-handler) block.

If you try to LEAVE a non-existing label, or if you try to LEAVE a HANDLER block, the following error will be produced:

```sql
ERROR 1308 (42000): LEAVE with no matching label: <label_name>
```

The following example uses `LEAVE` to exit the procedure if a condition is true:

```sql
CREATE PROCEDURE proc(IN p TINYINT)
CONTAINS SQL
`whole_proc`:
BEGIN
   SELECT 1;
   IF p < 1 THEN
      LEAVE `whole_proc`;
   END IF;
   SELECT 2;
END;

CALL proc(0);
+---+
| 1 |
+---+
| 1 |
+---+
```

## See Also

- [ITERATE](/programming-customizing-mariadb/programmatic-compound-statements/iterate) - Repeats a loop execution