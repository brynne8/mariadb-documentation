# Triggers and Implicit Locks

A [trigger](/programming-customizing-mariadb/triggers-events/triggers) may reference multiple tables, and if a [LOCK TABLES](/sql-statements-structure/sql-statements/transactions/lock-tables) statement is used on one of the tables, other tables may at the same time also implicitly be locked due to the trigger.

If the trigger only reads from the other table, that table will be read locked. If the trigger writes to the other table, it will be write locked. If a table is read-locked for reading via `LOCK TABLES`, but needs to be write-locked because it could be modified by a trigger, a write lock is taken.

All locks are acquired together when the `LOCK TABLES` statement is issued and they are released together on `UNLOCK TABLES`.

## Example

```sql
LOCK TABLE table1 WRITE
```

Assume `table1` contains the following trigger:

```sql
CREATE TRIGGER trigger1 AFTER INSERT ON table1 FOR EACH ROW
BEGIN
  INSERT INTO table2 VALUES (1);
  UPDATE table3 SET writes = writes+1
    WHERE id = NEW.id AND EXISTS (SELECT id FROM table4);
END;
```

Not only is `table1` write locked, `table2` and `table3` are also write locked, due to the possible [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert) and [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update), while `table4` is read locked due to the [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select).