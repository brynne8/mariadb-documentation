# Application-Time Periods

##### MariaDB starting with [10.4.3](/kb/en/mariadb-1043-release-notes/)

Support for application-time period-versioning was added in [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/).

Extending [system-versioned tables](/sql-statements-structure/temporal-tables/system-versioned-tables/), [MariaDB 10.4](/kb/en/what-is-mariadb-104/) supports application-time period tables. Time periods are defined by a range between two temporal columns. The columns must be of the same [temporal data type](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/), i.e. [DATE](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/date/), [TIMESTAMP](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/timestamp/) or [DATETIME](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/datetime/) ([TIME](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/time/) and [YEAR](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/year-data-type/) are  not supported), and of the same width.

Using time periods implicitly defines the two columns as `NOT NULL`.  It also adds a constraint to check whether the first value is less than the second value.  The constraint is invisible to [SHOW CREATE TABLE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-table/) statements.  The name of this constraint is prefixed by the time period name, to avoid conflict with other constraints.

### Creating Tables with Time Periods

To create a table with a time period, use a [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/) statement with the `PERIOD` table option.

```sql
CREATE TABLE t1(
   name VARCHAR(50), 
   date_1 DATE,
   date_2 DATE,
   PERIOD FOR date_period(date_1, date_2));

```

This creates a table with a `time_period` period and populates the table with some basic temporal values.

Examples are available in the MariaDB Server source code, at `mysql-test/suite/period/r/create.result`.

### Adding and Removing Time Periods

The [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/) statement now supports syntax for adding and removing time periods from a table. To add a period, use the `ADD PERIOD` clause.

For example:

```sql
CREATE OR REPLACE TABLE rooms (
 room_number INT,
 guest_name VARCHAR(255),
 checkin DATE,
 checkout DATE
 );

ALTER TABLE rooms ADD PERIOD FOR p(checkin,checkout);
```

To remove a period, use the `DROP PERIOD` clause:

```sql
ALTER TABLE rooms DROP PERIOD FOR p;
```

Both `ADD PERIOD` and `DROP PERIOD` clauses include an option to handle whether the period already exists:

```sql
ALTER TABLE rooms ADD PERIOD IF NOT EXISTS FOR p(checkin,checkout);

ALTER TABLE rooms DROP PERIOD IF EXISTS FOR p;
```

### Deletion by Portion

You can also remove rows that fall within certain time periods.

When MariaDB executes a `DELETE FOR PORTION` statement, it removes the row:

- When the row period falls completely within the delete period, it removes the row.
- When the row period overlaps the delete period, it shrinks the row, removing the overlap from the first or second row period value.
- When the delete period falls completely within the row period, it splits the row into two rows.  The first row runs from the starting row period to the starting delete period.  The second runs from the ending delete period to the ending row period.

To test this, first populate the table with some data to operate on:

```sql
CREATE TABLE t1(
   name VARCHAR(50), 
   date_1 DATE,
   date_2 DATE,
   PERIOD FOR date_period(date_1, date_2));

INSERT INTO t1 (name, date_1, date_2) VALUES
    ('a', '1999-01-01', '2000-01-01'),
    ('b', '1999-01-01', '2018-12-12'),
    ('c', '1999-01-01', '2017-01-01'),
    ('d', '2017-01-01', '2019-01-01');

SELECT * FROM t1;
+------+------------+------------+
| name | date_1     | date_2     |
+------+------------+------------+
| a    | 1999-01-01 | 2000-01-01 |
| b    | 1999-01-01 | 2018-12-12 |
| c    | 1999-01-01 | 2017-01-01 |
| d    | 2017-01-01 | 2019-01-01 |
+------+------------+------------+
```

Then, run the `DELETE FOR PORTION` statement:

```sql
DELETE FROM t1
FOR PORTION OF date_period
    FROM '2001-01-01' TO '2018-01-01';
Query OK, 3 rows affected (0.028 sec)

SELECT * FROM t1 ORDER BY name;
+------+------------+------------+
| name | date_1     | date_2     |
+------+------------+------------+
| a    | 1999-01-01 | 2000-01-01 |
| b    | 1999-01-01 | 2001-01-01 |
| b    | 2018-01-01 | 2018-12-12 |
| c    | 1999-01-01 | 2001-01-01 |
| d    | 2018-01-01 | 2019-01-01 |
+------+------------+------------+
```

Here:

- <em>a</em> is unchanged, as the range falls entirely out of the specified portion to be deleted.
- <em>b</em>, with values ranging from 1999 to 2018, is split into two rows, 1999 to 2000 and 2018-01 to 2018-12.
- <em>c</em>, with values ranging from 1999 to 2017, where only the upper value falls within the portion to be deleted, has been shrunk to 1999 to 2001.
- <em>d</em>, with values ranging from 2017 to 2019, where only the lower value falls within the portion to be deleted, has been shrunk to 2018 to 2019.

The `DELETE FOR PORTION` statement has the following restrictions

- The `FROM...TO` clause must be constant
- Multi-delete is not supported

If there are `DELETE` or `INSERT` triggers, it works as follows: any matched row is deleted, and then one or two rows are inserted. If the record is deleted completely, nothing is inserted.

### Updating by Portion

The [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update/) syntax now supports `UPDATE FOR PORTION`, which modifies rows based on their occurrence in a range:

To test it, first populate the table with some data:

```sql
TRUNCATE t1;

INSERT INTO t1 (name, date_1, date_2) VALUES
    ('a', '1999-01-01', '2000-01-01'),
    ('b', '1999-01-01', '2018-12-12'),
    ('c', '1999-01-01', '2017-01-01'),
    ('d', '2017-01-01', '2019-01-01');

SELECT * FROM t1;
+------+------------+------------+
| name | date_1     | date_2     |
+------+------------+------------+
| a    | 1999-01-01 | 2000-01-01 |
| b    | 1999-01-01 | 2018-12-12 |
| c    | 1999-01-01 | 2017-01-01 |
| d    | 2017-01-01 | 2019-01-01 |
+------+------------+------------+
```

Then run the update:

```sql
UPDATE t1 FOR PORTION OF date_period
  FROM '2000-01-01' TO '2018-01-01' 
SET name = CONCAT(name,'_original');

SELECT * FROM t1 ORDER BY name;
+------------+------------+------------+
| name       | date_1     | date_2     |
+------------+------------+------------+
| a          | 1999-01-01 | 2000-01-01 |
| b          | 1999-01-01 | 2000-01-01 |
| b          | 2018-01-01 | 2018-12-12 |
| b_original | 2000-01-01 | 2018-01-01 |
| c          | 1999-01-01 | 2000-01-01 |
| c_original | 2000-01-01 | 2017-01-01 |
| d          | 2018-01-01 | 2019-01-01 |
| d_original | 2017-01-01 | 2018-01-01 |
+------------+------------+------------+
```

- <em>a</em> is unchanged, as the range falls entirely out of the specified portion to be deleted.
- <em>b</em>, with values ranging from 1999 to 2018, is split into two rows, 1999 to 2000 and 2018-01 to 2018-12.
- <em>c</em>, with values ranging from 1999 to 2017, where only the upper value falls within the portion to be deleted, has been shrunk to 1999 to 2001.
- <em>d</em>, with values ranging from 2017 to 2019, where only the lower value falls within the portion to be deleted, has been shrunk to 2018 to 2019.
- Original rows affected by the update have "_original" appended to the name.

The `UPDATE FOR PORTION` statement has the following limitations:

- The operation cannot modify the two temporal columns used by the time period
- The operation cannot reference period values in the `SET` expression
- `FROM...TO` expressions must be constant

### WITHOUT OVERLAPS

##### MariaDB starting with [10.5.3](/kb/en/mariadb-1053-release-notes/)

[MariaDB 10.5](/kb/en/what-is-mariadb-105/) introduced a new clause, `WITHOUT OVERLAPS`, which allows one to create an index specifying that application time periods should not overlap.

An index constrained by `WITHOUT OVERLAPS` is required to be either a primary key or a unique index.

Take the following example, an application time period table for a booking system:

```sql
CREATE OR REPLACE TABLE rooms (
 room_number INT,
 guest_name VARCHAR(255),
 checkin DATE,
 checkout DATE,
 PERIOD FOR p(checkin,checkout)
 );

INSERT INTO rooms VALUES 
 (1, 'Regina', '2020-10-01', '2020-10-03'),
 (2, 'Cochise', '2020-10-02', '2020-10-05'),
 (1, 'Nowell', '2020-10-03', '2020-10-07'),
 (2, 'Eusebius', '2020-10-04', '2020-10-06');
```

Our system is not intended to permit overlapping bookings, so the fourth record above should not have been inserted. Using `WITHOUT OVERLAPS` in a unique index (in this case based on a combination of room number and the application time period) allows us to specify this constraint in the table definition.

```sql
CREATE OR REPLACE TABLE rooms (
 room_number INT,
 guest_name VARCHAR(255),
 checkin DATE,
 checkout DATE,
 PERIOD FOR p(checkin,checkout),
 UNIQUE (room_number, p WITHOUT OVERLAPS)
 );

INSERT INTO rooms VALUES 
 (1, 'Regina', '2020-10-01', '2020-10-03'),
 (2, 'Cochise', '2020-10-02', '2020-10-05'),
 (1, 'Nowell', '2020-10-03', '2020-10-07'),
 (2, 'Eusebius', '2020-10-04', '2020-10-06');
ERROR 1062 (23000): Duplicate entry '2-2020-10-06-2020-10-04' for key 'room_number'
```

### Further Examples

The implicit change from NULL to NOT NULL:

```sql
CREATE TABLE `t2` (
  `id` int(11) DEFAULT NULL,
  `d1` datetime DEFAULT NULL,
  `d2` datetime DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=latin1;

ALTER TABLE t2 ADD PERIOD FOR p(d1,d2);

SHOW CREATE TABLE t2\G
*************************** 1. row ***************************
       Table: t2
Create Table: CREATE TABLE `t2` (
  `id` int(11) DEFAULT NULL,
  `d1` datetime NOT NULL,
  `d2` datetime NOT NULL,
  PERIOD FOR `p` (`d1`, `d2`)
) ENGINE=InnoDB DEFAULT CHARSET=latin1
```

Due to this constraint, trying to add a time period where null data already exists will fail.

```sql
CREATE OR REPLACE TABLE `t2` (
  `id` int(11) DEFAULT NULL,
  `d1` datetime DEFAULT NULL,
  `d2` datetime DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=latin1;

INSERT INTO t2(id) VALUES(1);

ALTER TABLE t2 ADD PERIOD FOR p(d1,d2);
ERROR 1265 (01000): Data truncated for column 'd1' at row 1
```

### See Also

- [System-versioned Tables](/sql-statements-structure/temporal-tables/system-versioned-tables/)
- [Bitemporal Tables](/sql-statements-structure/temporal-tables/bitemporal-tables/)