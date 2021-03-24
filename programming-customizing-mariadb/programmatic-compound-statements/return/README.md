# RETURN

## Syntax

```sql
RETURN expr 
```

The `RETURN` statement terminates execution of a [stored function](/programming-customizing-mariadb/stored-routines/stored-functions/) and
returns the value <em>`expr`</em> to the function caller. There must be at least
one `RETURN` statement in a stored function. If the function has multiple exit points, all exit points must have a `RETURN`.

This statement is not used in [stored procedures](/programming-customizing-mariadb/stored-routines/stored-procedures/), [triggers](/programming-customizing-mariadb/triggers-events/triggers/), or [events](/programming-customizing-mariadb/triggers-events/event-scheduler/events/). [LEAVE](/programming-customizing-mariadb/programmatic-compound-statements/leave/) can be used instead.

The following example shows that `RETURN` can return the result of a [scalar subquery](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/joins-subqueries/subqueries/subqueries-scalar-subqueries/):

```sql
CREATE FUNCTION users_count() RETURNS BOOL
   READS SQL DATA
BEGIN
   RETURN (SELECT COUNT(DISTINCT User) FROM mysql.user);
END;
```