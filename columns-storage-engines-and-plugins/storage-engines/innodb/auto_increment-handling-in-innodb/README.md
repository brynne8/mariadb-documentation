# AUTO_INCREMENT Handling in InnoDB

## AUTO_INCREMENT Lock Modes

The [innodb_autoinc_lock_mode](/kb/en/innodb-system-variables/#innodb_autoinc_lock_mode) system variable determines the lock mode when generating [AUTO_INCREMENT](/columns-storage-engines-and-plugins/data-types/auto_increment/) values for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) tables. These modes allow [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) to make significant performance optimizations in certain circumstances.

The [innodb_autoinc_lock_mode](/kb/en/innodb-system-variables/#innodb_autoinc_lock_mode) system variable may be removed in a future release. See [MDEV-19577](https://jira.mariadb.org/browse/MDEV-19577) for more information.

### Traditional Lock Mode

When [innodb_autoinc_lock_mode](/kb/en/innodb-system-variables/#innodb_autoinc_lock_mode) is set to `0`, [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) uses the traditional lock mode.

In this mode, [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) holds a table-level lock for all [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert/) statements until the statement completes.

### Consecutive Lock Mode

When [innodb_autoinc_lock_mode](/kb/en/innodb-system-variables/#innodb_autoinc_lock_mode) is set to `1`, [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) uses the consecutive lock mode.

In this mode, [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) holds a table-level lock for all bulk [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert/) statements (such as [LOAD DATA](/kb/en/load-data-infile/) or [INSERT ... SELECT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-select/)) until the end of the statement. For simple [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert/) statements, no table-level lock is held. Instead, a lightweight mutex is used which scales significantly better. This is the default setting.

### Interleaved Lock Mode

When [innodb_autoinc_lock_mode](/kb/en/innodb-system-variables/#innodb_autoinc_lock_mode) is set to `2`, [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) uses the interleaved lock mode.

In this mode, [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) does not hold any table-level locks at all. This is the fastest and most scalable mode, but is not safe for [statement-based](/kb/en/binary-log-formats/#statement-based) replication.

## Setting AUTO_INCREMENT Values

The [AUTO_INCREMENT](/columns-storage-engines-and-plugins/data-types/auto_increment/) value for an [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) table can be set for a table by executing the [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/) statement and specifying the [AUTO_INCREMENT](/kb/en/create-table/#auto_increment) table option. For example:

```sql
ALTER TABLE tab AUTO_INCREMENT=100;
```

However, in [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/) and before, [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) stores the table's [AUTO_INCREMENT](/columns-storage-engines-and-plugins/data-types/auto_increment/) counter in memory. In these versions, when the server restarts, the counter is re-initialized to the highest value found in the table. This means that the above operation can be undone if the server is restarted before any rows are written to the table.

In [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/) and later, the [AUTO_INCREMENT](/columns-storage-engines-and-plugins/data-types/auto_increment/) counter is persistent, so this restriction is no longer present. Persistent, however, does not mean transactional. Gaps may still occur in some cases, such as if a [INSERT IGNORE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-ignore/) statement fails, or if a user executes [ROLLBACK](/sql-statements-structure/sql-statements/transactions/rollback/) or [ROLLBACK TO SAVEPOINT](/sql-statements-structure/sql-statements/transactions/savepoint/).

For example:

```sql
CREATE TABLE t1 (pk INT AUTO_INCREMENT PRIMARY KEY, i INT, UNIQUE (i)) ENGINE=InnoDB;

INSERT INTO t1 (i) VALUES (1),(2),(3);
INSERT IGNORE INTO t1 (pk, i) VALUES (100,1);
Query OK, 0 rows affected, 1 warning (0.099 sec)

SELECT * FROM t1;
+----+------+
| pk | i    |
+----+------+
|  1 |    1 |
|  2 |    2 |
|  3 |    3 |
+----+------+

SHOW CREATE TABLE t1\G
*************************** 1. row ***************************
       Table: t1
Create Table: CREATE TABLE `t1` (
  `pk` int(11) NOT NULL AUTO_INCREMENT,
  `i` int(11) DEFAULT NULL,
  PRIMARY KEY (`pk`),
  UNIQUE KEY `i` (`i`)
) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=latin1
```

If the server is restarted at this point, then the [AUTO_INCREMENT](/columns-storage-engines-and-plugins/data-types/auto_increment/) counter will revert to `101`, which is the persistent value set as part of the failed [INSERT IGNORE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-ignore/).

```sql
# Restart server
SHOW CREATE TABLE t1\G
*************************** 1. row ***************************
       Table: t1
Create Table: CREATE TABLE `t1` (
  `pk` int(11) NOT NULL AUTO_INCREMENT,
  `i` int(11) DEFAULT NULL,
  PRIMARY KEY (`pk`),
  UNIQUE KEY `i` (`i`)
) ENGINE=InnoDB AUTO_INCREMENT=101 DEFAULT CHARSET=latin1
```

## See Also

- [AUTO_INCREMENT](/columns-storage-engines-and-plugins/data-types/auto_increment/)
- [AUTO_INCREMENT FAQ](/kb/en/autoincrement-faq/)
- [LAST_INSERT_ID](/built-in-functions/secondary-functions/information-functions/last_insert_id/)
- [Sequences](/sql-statements-structure/sequences/) - an alternative to auto_increment available from [MariaDB 10.3](/kb/en/what-is-mariadb-103/)