# Instant ADD COLUMN for InnoDB

##### MariaDB starting with [10.3.2](/kb/en/mariadb-1032-release-notes/)

Instant <a undefined>ALTER TABLE ... ADD COLUMN</a> for InnoDB was introduced in [MariaDB 10.3.2](/kb/en/mariadb-1032-release-notes/). The `INSTANT` option for the <a undefined>ALGORITHM</a> clause was introduced in [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/).

Normally, adding a column to a table requires the full table to be rebuilt. The complexity of the operation is proportional to the size of the table, or O(n·m) where n is the number of rows in the table and m is the number of indexes.

In [MariaDB 10.0](/kb/en/what-is-mariadb-100/) and later, the [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/) statement supports [online DDL](/kb/en/alter-table/#online-ddl) for storage engines that have implemented the relevant online DDL [algorithms](/kb/en/alter-table/#algorithm) and [locking strategies](/kb/en/alter-table/#lock).

The [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) storage engine has implemented online DDL for many operations. These online DDL optimizations allow concurrent DML to the table in many cases, even if the table needs to be rebuilt.

See [InnoDB Online DDL Overview](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-online-ddl/innodb-online-ddl-overview/) for more information about online DDL with InnoDB.

Allowing concurrent DML during the operation does not solve all problems. When a column was added to a table with the older in-place optimization, the resulting table rebuild could still significantly increase the I/O and memory consumption and cause replication lag.

In contrast, with the new instant <a undefined>ALTER TABLE ... ADD COLUMN</a>, all that is needed is an O(log n) operation to insert a special hidden record into the table, and an update of the data dictionary. For a large table, instead of taking several hours, the operation would be completed in the blink of an eye. The <a undefined>ALTER TABLE ... ADD COLUMN</a> operation is only slightly more expensive than a regular [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert/), due to locking constraints.

In the past, some developers may have implemented a kind of "instant add column" in the application by encoding multiple columns in a single [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text/) or [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob/) column. MariaDB [Dynamic Columns](/sql-statements-structure/nosql/dynamic-columns/) was an early example of that. A more recent example is [JSON](/built-in-functions/special-functions/json-functions/) and related string manipulation functions.

Adding real columns has the following advantages over encoding columns into a single "expandable" column:

- Efficient storage in a native binary format
- Data type safety
- Indexes can be built natively
- Constraints are available: UNIQUE, CHECK, FOREIGN KEY
- DEFAULT values can be specified
- Triggers can be written more easily

With instant <a undefined>ALTER TABLE ... ADD COLUMN</a>, you can enjoy all the benefits of structured storage without the drawback of having to rebuild the table.

Instant <a undefined>ALTER TABLE ... ADD COLUMN</a> is available for both old and new InnoDB tables. Basically you can just upgrade from MySQL 5.x or MariaDB and start adding columns instantly.

Columns instantly added to a table exist in a separate data structure from the main table definition, similar to how InnoDB separates `BLOB` columns.  If the table ever becomes empty, (such as from [TRUNCATE](/built-in-functions/numeric-functions/truncate/) or [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete/) statements), InnoDB incorporates the instantly added columns into the main table definition. See [InnoDB Online DDL Operations with ALGORITHM=INSTANT: Non-canonical Storage Format Caused by Some Operations](/kb/en/innodb-online-ddl-operations-with-algorithminstant/#non-canonical-storage-format-caused-by-some-operations) for more information.

The operation is also crash safe. If the server is killed while executing an instant <a undefined>ALTER TABLE ... ADD COLUMN</a>, when the table is restored InnoDB integrates the new column, flattening the table definition.

## Limitations

- In [MariaDB 10.3](/kb/en/what-is-mariadb-103/), instant <a undefined>ALTER TABLE ... ADD COLUMN</a> only applies when the added columns appear last in the table. The place specifier `LAST` is the default. If `AFTER` col is specified, then col must be the last column, or the operation will require the table to be rebuilt. In [MariaDB 10.4](/kb/en/what-is-mariadb-104/), this restriction has been lifted.

- If the table contains a hidden `FTS_DOC_ID` column due to a [FULLTEXT INDEX](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes/), then instant <a undefined>ALTER TABLE ... ADD COLUMN</a> will not be possible.

- InnoDB data files after instant <a undefined>ALTER TABLE ... ADD COLUMN</a> cannot be imported to older versions of MariaDB or MySQL without first being rebuilt.

- After using Instant <a undefined>ALTER TABLE ... ADD COLUMN</a>, any table-rebuilding operation such as <a undefined>ALTER TABLE … FORCE</a> will incorporate instantaneously added columns into the main table body.

- Instant <a undefined>ALTER TABLE ... ADD COLUMN</a> is not available for [ROW_FORMAT=COMPRESSED](/kb/en/xtradbinnodb-storage-formats/#compressed).

- In [MariaDB 10.3](/kb/en/what-is-mariadb-103/), <a undefined>ALTER TABLE … DROP COLUMN</a> requires the table to be rebuilt. In [MariaDB 10.4](/kb/en/what-is-mariadb-104/), this restriction has been lifted.

## Example

```sql
CREATE TABLE t(id INT PRIMARY KEY, u INT UNSIGNED NOT NULL UNIQUE)
ENGINE=InnoDB;

INSERT INTO t(id,u) VALUES(1,1),(2,2),(3,3);

ALTER TABLE t ADD COLUMN
(d DATETIME DEFAULT current_timestamp(),
 p POINT NOT NULL DEFAULT ST_GeomFromText('POINT(0 0)'),
 t TEXT CHARSET utf8 DEFAULT 'The quick brown fox jumps over the lazy dog');

UPDATE t SET t=NULL WHERE id=3;

SELECT id,u,d,ST_AsText(p),t FROM t;

SELECT variable_value FROM information_schema.global_status
WHERE variable_name = 'innodb_instant_alter_column';
```

The above example illustrates that when the added columns are declared NOT NULL, a DEFAULT value must be available, either implied by the data type or set explicitly by the user. The expression need not be constant, but it must not refer to the columns of the table, such as DEFAULT u+1 (a MariaDB extension). The DEFAULT current_timestamp() would be evaluated at the time of the ALTER TABLE and apply to each row, like it does for non-instant ALTER TABLE. If a subsequent ALTER TABLE changes the DEFAULT value for subsequent INSERT, the values of the columns in existing records will naturally be unaffected.

The design was brainstormed in April by engineers from MariaDB Corporation, Alibaba and Tencent. A prototype was developed by Vin Chen (陈福荣) from the Tencent Game DBA Team.