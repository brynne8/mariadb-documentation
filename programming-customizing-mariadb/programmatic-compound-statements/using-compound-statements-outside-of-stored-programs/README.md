# Using Compound Statements Outside of Stored Programs

##### MariaDB starting with [10.1.1](/kb/en/mariadb-1011-release-notes/)

Starting from [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/) compound statements can also be used outside of [stored programs](/programming-customizing-mariadb/stored-routines/).

```sql
delimiter |
IF @have_innodb THEN
  CREATE TABLE IF NOT EXISTS innodb_index_stats (
    database_name    VARCHAR(64) NOT NULL,
    table_name       VARCHAR(64) NOT NULL,
    index_name       VARCHAR(64) NOT NULL,
    last_update      TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    stat_name        VARCHAR(64) NOT NULL,
    stat_value       BIGINT UNSIGNED NOT NULL,
    sample_size      BIGINT UNSIGNED,
    stat_description VARCHAR(1024) NOT NULL,
    PRIMARY KEY (database_name, table_name, index_name, stat_name)
  ) ENGINE=INNODB DEFAULT CHARSET=utf8 COLLATE=utf8_bin STATS_PERSISTENT=0;
END IF|
Query OK, 0 rows affected, 2 warnings (0.00 sec)
```

Note, that using compound statements this way is subject to following limitations:

- Only [BEGIN](/programming-customizing-mariadb/programmatic-compound-statements/begin-end/), [IF](/kb/en/if-statement/), [CASE](/programming-customizing-mariadb/programmatic-compound-statements/case-statement/), [LOOP](/programming-customizing-mariadb/programmatic-compound-statements/loop/), [WHILE](/programming-customizing-mariadb/programmatic-compound-statements/while/), [REPEAT](/programming-customizing-mariadb/programmatic-compound-statements/repeat-loop/) statements may start a compound statement outside of stored programs.
- [BEGIN](/programming-customizing-mariadb/programmatic-compound-statements/begin-end/) must use the `BEGIN NOT ATOMIC` syntax (otherwise it'll be confused with [BEGIN](/sql-statements-structure/sql-statements/transactions/start-transaction/) that starts a transaction).
- A compound statement might not start with a label.
- A compound statement is parsed completely<span>—</span>note "2 warnings" in the above example, even if the condition was false (InnoDB was, indeed, disabled), and the [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/) statement was not executed, it was still parsed and the parser produced "Unknown storage engine" warning.

Inside a compound block first three limitations do not apply, one can use anything that can be used inside a stored program — including labels, condition handlers, variables, and so on:

```sql
BEGIN NOT ATOMIC
    DECLARE foo CONDITION FOR 1146;
    DECLARE x INT DEFAULT 0;
    DECLARE CONTINUE HANDLER FOR SET x=1;
    INSERT INTO test.t1 VALUES ("hndlr1", val, 2);
    END|
```

Example how to use `IF`:

```sql
IF (1>0) THEN BEGIN NOT ATOMIC SELECT 1; END ; END IF;;

```

Example of how to use `WHILE` loop:

```sql
DELIMITER |
BEGIN NOT ATOMIC
    DECLARE x INT DEFAULT 0;
    WHILE x <= 10 DO
        SET x = x + 1;
        SELECT x;
    END WHILE;
END|
DELIMITER ;
```