# AUTO_INCREMENT FAQ

## How do I get the last inserted auto_increment value?

Use the [LAST_INSERT_ID()](/built-in-functions/secondary-functions/information-functions/last_insert_id) function:

```sql
SELECT LAST_INSERT_ID();
```

## What if someone else inserts before I select my id?

[LAST_INSERT_ID()](/built-in-functions/secondary-functions/information-functions/last_insert_id) is connection specific, so there is no problem from race conditions.

## How do I get the next value to be inserted?

You don't. Insert, then find out what you did with [LAST_INSERT_ID()](/built-in-functions/secondary-functions/information-functions/last_insert_id).

## How do I change what number auto_increment starts with?

`ALTER TABLE yourTable AUTO_INCREMENT = x;` <span>—</span> Next insert will contain `x` or `MAX(autoField) + 1`, whichever is higher

or

`INSERT INTO yourTable (autoField) VALUES (x);` <span>—</span>
Next insert will contain `x+1` or `MAX(autoField) + 1`, whichever is higher

Issuing [TRUNCATE TABLE](/sql-statements-structure/sql-statements/table-statements/truncate-table) will delete all the rows in the table, and will reset
the auto_increment value to 0 in most cases (some earlier versions mapped
TRUNCATE to DELETE for InnoDB tables, meaning the auto_increment value would
not be reset).

## How do I renumber rows once I've deleted some in the middle?

Typically, you don't want to. Gaps are hardly ever a problem; if your
application can't handle gaps in the sequence, you probably should rethink your
application.

## Can I do group-wise auto_increment?

Yes, if you use the [MyISAM engine](/kb/en/myisam/).

## How do I get the auto_increment value in a BEFORE INSERT trigger?

You don't. It's only available after insert.

## How do I assign two fields the same auto_increment value in one query?

You can't, not even with an AFTER INSERT trigger. Insert, then go back and
update using `LAST_INSERT_ID()`. Those two statements could be
wrapped into one stored procedure if you wish.

However, you can mimic this behavior with a BEFORE INSERT trigger and a second
table to store the sequence position:

```sql
CREATE TABLE sequence (table_name VARCHAR(255), position INT UNSIGNED);
INSERT INTO sequence VALUES ('testTable', 0);
CREATE TABLE testTable (firstAuto INT UNSIGNED, secondAuto INT UNSIGNED);
DELIMITER //
CREATE TRIGGER testTable_BI BEFORE INSERT ON testTable FOR EACH ROW BEGIN
  UPDATE sequence SET position = LAST_INSERT_ID(position + 1) WHERE table_name = 'testTable';
  SET NEW.firstAuto = LAST_INSERT_ID();
  SET NEW.secondAuto = LAST_INSERT_ID();
END//
DELIMITER ;
INSERT INTO testTable VALUES (NULL, NULL), (NULL, NULL);
SELECT * FROM testTable;

+-----------+------------+
| firstAuto | secondAuto |
+-----------+------------+
|         1 |          1 |
|         2 |          2 |
+-----------+------------+
```

The same sequence table can maintain separate sequences for multiple tables (or
separate sequences for different fields in the same table) by adding extra
rows.

## Does the auto_increment field have to be primary key?

No, it only has to be indexed. It doesn't even have to be unique.

## InnoDB and AUTO_INCREMENT

See [AUTO_INCREMENT handling in XtraDB/InnoDB](/kb/en/auto_increment-handling-in-xtradbinnodb/)

## General Information To Read

[AUTO_INCREMENT](/columns-storage-engines-and-plugins/data-types/auto_increment)

## Manual Notes

There can be only one `AUTO_INCREMENT` column per table, it must be indexed,
and it cannot have a `DEFAULT` value. An `AUTO_INCREMENT` column works
properly only if it contains only positive values. Inserting a negative number
is regarded as inserting a very large positive number. This is done to avoid
precision problems when numbers wrap over from positive to negative and also to
ensure that you do not accidentally get an `AUTO_INCREMENT` column that
contains 0.

## How to start a table with a set AUTO_INCREMENT value?

```sql
CREATE TABLE autoinc_test (
  h INT UNSIGNED PRIMARY KEY AUTO_INCREMENT, 
  m INT UNSIGNED 
) AUTO_INCREMENT = 100;

INSERT INTO autoinc_test ( m ) VALUES ( 1 );

SELECT * FROM autoinc_test;
+-----+------+
| h   | m    |
+-----+------+
| 100 |    1 |
+-----+------+
```

## See Also

- [AUTO_INCREMENT](/columns-storage-engines-and-plugins/data-types/auto_increment)
- [AUTO_INCREMENT handling in XtraDB/InnoDB](/kb/en/auto_increment-handling-in-xtradbinnodb/)
- [LAST_INSERT_ID()](/built-in-functions/secondary-functions/information-functions/last_insert_id)
- [BLACKHOLE and AUTO_INCREMENT](/kb/en/blackhole/#blackhole-and-auto_increment)
- [Sequences](/sql-statements-structure/sequences) - an alternative to auto_increment available from [MariaDB 10.3](/kb/en/what-is-mariadb-103/)

<em>The initial version of this article was copied, with permission, from [http://hashmysql.org/wiki/Autoincrement_FAQ](http://hashmysql.org/wiki/Autoincrement_FAQ) on 2012-10-05.</em>