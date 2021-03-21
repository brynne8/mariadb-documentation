# INSERT IGNORE

## Ignoring Errors

Normally [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert/) stops and rolls back when it encounters an error.

By using the [IGNORE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/ignore/) keyword all errors are converted to warnings, which will not stop inserts of additional rows.

The IGNORE and DELAYED options are ignored when you use [ON DUPLICATE KEY UPDATE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-on-duplicate-key-update/).

## Incompatibilities

##### MariaDB until [5.5.28](/kb/en/mariadb-5528-release-notes/)

- MySQL and MariaDB before 5.5.28 didn't give warnings for duplicate key errors when using <code class="highlight fixed" style="white-space:pre-wrap">IGNORE</code>.
You can get the old behaviour if you set [OLD_MODE](/kb/en/old_mode/) to <code class="highlight fixed" style="white-space:pre-wrap">NO_DUP_KEY_WARNINGS_WITH_IGNORE</code>

## Examples

```sql
CREATE TABLE t1 (x INT UNIQUE);

INSERT INTO t1 VALUES(1),(2);

INSERT INTO t1 VALUES(2),(3);
ERROR 1062 (23000): Duplicate entry '2' for key 'x'
SELECT * FROM t1;
+------+
| x    |
+------+
|    1 |
|    2 |
+------+
2 rows in set (0.00 sec)

INSERT IGNORE INTO t1 VALUES(2),(3);
Query OK, 1 row affected, 1 warning (0.04 sec)

SHOW WARNINGS;
+---------+------+---------------------------------+
| Level   | Code | Message                         |
+---------+------+---------------------------------+
| Warning | 1062 | Duplicate entry '2' for key 'x' |
+---------+------+---------------------------------+

SELECT * FROM t1;
+------+
| x    |
+------+
|    1 |
|    2 |
|    3 |
+------+
```

See [INSERT ON DUPLICATE KEY UPDATE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-on-duplicate-key-update/) for further examples using that syntax.

## See Also

- [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert/)
- [INSERT DELAYED](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-delayed/)
- [INSERT SELECT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-select/)
- [HIGH_PRIORITY and LOW_PRIORITY](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/high_priority-and-low_priority/)
- [Concurrent Inserts](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/concurrent-inserts/)
- [INSERT - Default &amp; Duplicate Values](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-default-duplicate-values/)
- [INSERT IGNORE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-ignore/)
- [INSERT ON DUPLICATE KEY UPDATE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-on-duplicate-key-update/)