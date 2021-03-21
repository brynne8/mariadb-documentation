# AUTO_INCREMENT

## Description

The `AUTO_INCREMENT` attribute can be used to generate a unique identity for new rows. When you insert a new record to the table, and the auto_increment field is [NULL](/columns-storage-engines-and-plugins/data-types/null-values) or DEFAULT, the value will automatically be incremented. This also applies to 0, unless the `NO_AUTO_VALUE_ON_ZERO` [SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode) is enabled.

`AUTO_INCREMENT` columns start from 1 by default. The automatically generated value can never be lower than 0.

Each table can have only one `AUTO_INCREMENT` column. It must defined as a key (not necessarily the `PRIMARY KEY` or `UNIQUE` key). In some storage engines (including the default [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb)), if the key consists of multiple columns, the `AUTO_INCREMENT` column must be the first column. Storage engines that permit the column to be placed elsewhere are [Aria](/columns-storage-engines-and-plugins/storage-engines/aria), [MyISAM](/kb/en/myisam/), [MERGE](/columns-storage-engines-and-plugins/storage-engines/merge), [Spider](/columns-storage-engines-and-plugins/storage-engines/spider), [TokuDB](/columns-storage-engines-and-plugins/storage-engines/tokudb), [BLACKHOLE](/columns-storage-engines-and-plugins/storage-engines/blackhole), [FederatedX](/kb/en/federatedx/) and [Federated](/columns-storage-engines-and-plugins/storage-engines/legacy-storage-engines/federated-storage-engine).

```sql
CREATE TABLE animals (
     id MEDIUMINT NOT NULL AUTO_INCREMENT,
     name CHAR(30) NOT NULL,
     PRIMARY KEY (id)
 );

INSERT INTO animals (name) VALUES
    ('dog'),('cat'),('penguin'),
    ('fox'),('whale'),('ostrich');
```

```sql
SELECT * FROM animals;
+----+---------+
| id | name    |
+----+---------+
|  1 | dog     |
|  2 | cat     |
|  3 | penguin |
|  4 | fox     |
|  5 | whale   |
|  6 | ostrich |
+----+---------+
```

`SERIAL` is an alias for `BIGINT UNSIGNED NOT NULL AUTO_INCREMENT UNIQUE`.

```sql
CREATE TABLE t (id SERIAL, c CHAR(1)) ENGINE=InnoDB;

SHOW CREATE TABLE t \G
*************************** 1. row ***************************
       Table: t
Create Table: CREATE TABLE `t` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `c` char(1) DEFAULT NULL,
  UNIQUE KEY `id` (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=latin1
```

## Setting or Changing the Auto_Increment Value

You can use an [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table) statement to assign a new value to the `auto_increment` table option, or set the [insert_id](/kb/en/server-system-variables/#insert_id) server system variable to change the next `AUTO_INCREMENT` value inserted by the current session.

[LAST_INSERT_ID()](/built-in-functions/secondary-functions/information-functions/last_insert_id) can be used to see the last `AUTO_INCREMENT` value inserted by the current session.

```sql
ALTER TABLE animals AUTO_INCREMENT=8;

INSERT INTO animals (name) VALUES ('aardvark');

SELECT * FROM animals;
+----+-----------+
| id | name      |
+----+-----------+
|  1 | dog       |
|  2 | cat       |
|  3 | penguin   |
|  4 | fox       |
|  5 | whale     |
|  6 | ostrich   |
|  8 | aardvark  |
+----+-----------+

SET insert_id=12;

INSERT INTO animals (name) VALUES ('gorilla');

SELECT * FROM animals;
+----+-----------+
| id | name      |
+----+-----------+
|  1 | dog       |
|  2 | cat       |
|  3 | penguin   |
|  4 | fox       |
|  5 | whale     |
|  6 | ostrich   |
|  8 | aardvark  |
| 12 | gorilla   |
+----+-----------+
```

## InnoDB/XtraDB

Until [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/), InnoDB and XtraDB used an auto-increment counter that is stored in memory. When the server restarts, the counter is re-initialized to the highest value used in the table, which cancels the effects of any AUTO_INCREMENT = N option in the table statements.

From [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/), this restriction has been lifted and AUTO_INCREMENT is persistent.

See also [AUTO_INCREMENT Handling in XtraDB/InnoDB](/kb/en/auto_increment-handling-in-xtradbinnodb/).

## Setting Explicit Values

It is possible to specify a value for an `AUTO_INCREMENT` column. If the key is primary or unique, the value must not already exist in the key.

If the new value is higher than the current maximum value, the `AUTO_INCREMENT` value is updated, so the next value will be higher. If the new value is lower than the current maximum value,  the `AUTO_INCREMENT` value remains unchanged.

The following example demonstrates these behaviors:

```sql
CREATE TABLE t (id INTEGER UNSIGNED AUTO_INCREMENT PRIMARY KEY) ENGINE = InnoDB;

INSERT INTO t VALUES (NULL);
SELECT id FROM t;
+----+
| id |
+----+
|  1 |
+----+

INSERT INTO t VALUES (10); -- higher value
SELECT id FROM t;
+----+
| id |
+----+
|  1 |
| 10 |
+----+

INSERT INTO t VALUES (2); -- lower value
INSERT INTO t VALUES (NULL); -- auto value
SELECT id FROM t;
+----+
| id |
+----+
|  1 |
|  2 |
| 10 |
| 11 |
+----+
```

The [ARCHIVE](/columns-storage-engines-and-plugins/storage-engines/archive) storage engine does not allow to insert a value that is lower than the current maximum.

## Missing Values

An AUTO_INCREMENT column normally has missing values. This happens because if a row is deleted, or an AUTO_INCREMENT value is explicitly updated, old values are never re-used. The REPLACE statement also deletes a row, and its value is wasted. With InnoDB, values can be reserved by a transaction; but if the transaction fails (for example, because of a ROLLBACK) the reserved value will be lost.

Thus AUTO_INCREMENT values can be used to sort results in a chronological order, but not to create a numeric sequence.

## Replication

To make master-master or Galera safe to use `AUTO_INCREMENT` one should use the system variables 
 [auto_increment_increment](/kb/en/replication-and-binary-log-server-system-variables/#auto_increment_increment) and [auto_increment_offset](/kb/en/replication-and-binary-log-server-system-variables/#auto_increment_offset) to generate unique values for each server.

## CHECK Constraints, DEFAULT Values and Virtual Columns

##### MariaDB starting with [10.2.6](/kb/en/mariadb-1026-release-notes/)

From [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/) auto_increment columns are no longer permitted in [CHECK constraints](/sql-statements-structure/sql-statements/data-definition/constraint), [DEFAULT value expressions](/kb/en/create-table/#default) and [virtual columns](/kb/en/virtual-computed-columns/). They were permitted in earlier versions, but did not work correctly. See [MDEV-11117](https://jira.mariadb.org/browse/MDEV-11117).

## See Also

- [Getting Started with Indexes](/replication/optimization-and-tuning/optimization-and-indexes/getting-started-with-indexes)
- [Sequences](/sql-statements-structure/sequences) - an alternative to auto_increment available from [MariaDB 10.3](/kb/en/what-is-mariadb-103/)
- [AUTO_INCREMENT FAQ](/kb/en/autoincrement-faq/)
- [LAST_INSERT_ID()](/built-in-functions/secondary-functions/information-functions/last_insert_id)
- [AUTO_INCREMENT handling in XtraDB/InnoDB](/kb/en/auto_increment-handling-in-xtradbinnodb/)
- [BLACKHOLE and AUTO_INCREMENT](/kb/en/blackhole/#blackhole-and-auto_increment)
- [UUID_SHORT()](/built-in-functions/secondary-functions/miscellaneous-functions/uuid_short) - Generate unique ids
- [Generating Identifiers – from AUTO_INCREMENT to Sequence (percona.com)](https://www.percona.com/community-blog/2018/10/12/generating-identifiers-auto_increment-sequence/)