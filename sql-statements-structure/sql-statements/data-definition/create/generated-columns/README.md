# Generated (Virtual and Persistent/Stored) Columns

## Syntax

```sql
<type>  [GENERATED ALWAYS]  AS   ( <expression> )
[VIRTUAL | PERSISTENT | STORED]  [UNIQUE] [UNIQUE KEY] [COMMENT <text>]
```

MariaDB's generated columns syntax is designed to be similar to the syntax for [Microsoft SQL Server's computed columns](https://docs.microsoft.com/en-us/sql/relational-databases/tables/specify-computed-columns-in-a-table?view=sql-server-2017) and [Oracle Database's virtual columns](https://oracle-base.com/articles/11g/virtual-columns-11gr1). In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, the syntax is also compatible with the syntax for [MySQL's generated columns](https://dev.mysql.com/doc/refman/5.7/en/create-table-generated-columns.html).

## Description

A generated column is a column in a table that cannot explicitly be set to a specific value in a [DML query](/sql-statements-structure/sql-statements/data-manipulation/). Instead, its value is automatically generated based on an expression. This expression might generate the value based on the values of other columns in the table, or it might generate the value by calling [built-in functions](/built-in-functions/) or [user-defined functions (UDFs)](/programming-customizing-mariadb/user-defined-functions/).

There are two types of generated columns:

- `PERSISTENT` (a.k.a. `STORED`): This type's value is actually stored in the table.
- `VIRTUAL`: This type's value is not stored at all. Instead, the value is generated dynamically when the table is queried. This type is the default.

Generated columns are also sometimes called computed columns or virtual columns.

## Supported Features

### Storage Engine Support

- Generated columns can only be used with storage engines which support them. If you try to use a storage engine that does not support them, then you will see an error similar to the following:

```sql
ERROR 1910 (HY000): TokuDB storage engine does not support computed columns
```

- [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/), [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/), [MyISAM](/kb/en/myisam/) and [CONNECT](/columns-storage-engines-and-plugins/storage-engines/connect/using-connect/using-connect-virtual-and-special-columns/) support generated columns.

- A column in a [MERGE](/columns-storage-engines-and-plugins/storage-engines/merge/) table can be built on a `PERSISTENT` generated column.
<ul start="1"><li>However, a column in a MERGE table can <strong>not</strong> be defined as a `VIRTUAL` and `PERSISTENT` generated column.
</li></ul>

### Data Type Support

- All data types are supported when defining generated columns.

- Using the <a undefined>ZEROFILL</a> column option is supported when defining generated columns.

##### MariaDB starting with [10.2.6](/kb/en/mariadb-1026-release-notes/)

In [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/) and later, the following statements apply to data types for generated columns:

- Using the [AUTO_INCREMENT](/columns-storage-engines-and-plugins/data-types/auto_increment/) column option is <strong>not</strong> supported when defining generated columns. Previously, it was supported, but this support was removed, because it would not work correctly. See [MDEV-11117](https://jira.mariadb.org/browse/MDEV-11117).

### Index Support

- Using a generated column as a table's primary key is <strong>not</strong> supported. See [MDEV-5590](https://jira.mariadb.org/browse/MDEV-5590) for more information. If you try to use one as a primary key, then you will see an error similar to the following:

```sql
ERROR 1903 (HY000): Primary key cannot be defined upon a computed column
```

- Using `PERSISTENT` generated columns as part of a [foreign key](/replication/optimization-and-tuning/optimization-and-indexes/foreign-keys/) is supported.

- Referencing `PERSISTENT` generated columns as part of a [foreign key](/replication/optimization-and-tuning/optimization-and-indexes/foreign-keys/) is also supported.
<ul start="1"><li>However, using the `ON UPDATE CASCADE`, `ON UPDATE SET NULL`, or `ON DELETE SET NULL` clauses is <strong>not</strong> supported. If you try to use an unsupported clause, then you will see an error similar to the following:
</li></ul>

```sql
ERROR 1905 (HY000): Cannot define foreign key with ON UPDATE SET NULL clause on a computed column
```

##### MariaDB starting with [10.2.3](/kb/en/mariadb-1023-release-notes/)

In [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/) and later, the following statements apply to indexes for generated columns:

- Defining indexes on both `VIRTUAL` and `PERSISTENT` generated columns is supported.
<ul start="1"><li>If an index is defined on a generated column, then the optimizer considers using it in the same way as indexes based on "real" columns.
</li></ul>

##### MariaDB until [10.2.2](/kb/en/mariadb-1022-release-notes/)

In [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/) and before, the following statements apply to indexes for generated columns:

- Defining indexes on `VIRTUAL` generated columns is <strong>not</strong> supported.

- Defining indexes on `PERSISTENT` generated columns is supported.
<ul start="1"><li>If an index is defined on a generated column, then the optimizer considers using it in the same way as indexes based on "real" columns.
</li></ul>

### Statement Support

- Generated columns are used in [DML queries](/sql-statements-structure/sql-statements/data-manipulation/) just as if they were "real" columns.
<ul start="1"><li>However, `VIRTUAL` and `PERSISTENT` generated columns differ in how their data is stored.
<ul start="1"><li>Values for `PERSISTENT` generated columns are generated whenever a [DML queries](/sql-statements-structure/sql-statements/data-manipulation/) inserts or updates the row with the special `DEFAULT` value. This generates the columns value, and it is stored in the table like the other "real" columns. This value can be read by other [DML queries](/sql-statements-structure/sql-statements/data-manipulation/) just like the other "real" columns.
</li><li>Values for `VIRTUAL` generated columns are not stored in the table. Instead, the value is generated dynamically whenever the column is queried. If other columns in a row are queried, but the `VIRTUAL` generated column is not one of the queried columns, then the column's value is not generated.
</li></ul>
</li></ul>

- The [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/) statement supports generated columns.

- Generated columns can be referenced in the [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert/), [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update/), and [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete/) statements.
<ul start="1"><li>However, `VIRTUAL` or `PERSISTENT` generated columns cannot be explicitly set to any other values than `NULL` or [DEFAULT](/built-in-functions/secondary-functions/information-functions/default/). If a generated column is explicitly set to any other value, then the outcome depends on whether [strict mode](/kb/en/sql-mode/#strict-mode) is enabled in [sql_mode](/mariadb-administration/variables-and-modes/sql-mode/). If it is not enabled, then a warning will be raised and the default generated value will be used instead. If it is enabled, then an error will be raised instead.
</li></ul>

- The [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/) statement has limited support for generated columns.
<ul start="1"><li>It supports defining generated columns in a new table.
</li><li>It supports using generated columns to [partition tables](/mariadb-administration/partitioning-tables/).
</li><li>It does <strong>not</strong> support using the [versioning clauses](/sql-statements-structure/temporal-tables/system-versioned-tables/) with generated columns.
</li></ul>

- The [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/) statement has limited support for generated columns.
<ul start="1"><li>It supports the `MODIFY` and `CHANGE` clauses for `PERSISTENT` generated columns.
</li><li>It does <strong>not</strong> support the `MODIFY` clause for `VIRTUAL` generated columns if <a undefined>ALGORITHM</a> is not set to `COPY`. See [MDEV-15476](https://jira.mariadb.org/browse/MDEV-15476) for more information.
</li><li>It does <strong>not</strong> support the  `CHANGE` clause for `VIRTUAL` generated columns if <a undefined>ALGORITHM</a> is not set to `COPY`. See [MDEV-17035](https://jira.mariadb.org/browse/MDEV-17035) for more information.
</li><li>It does <strong>not</strong> support altering a table if <a undefined>ALGORITHM</a> is not set to `COPY` if the table has a `VIRTUAL` generated column that is indexed. See [MDEV-14046](https://jira.mariadb.org/browse/MDEV-14046) for more information.
</li><li>It does <strong>not</strong> support adding a `VIRTUAL` generated column with the `ADD` clause if the same statement is also adding other columns if <a undefined>ALGORITHM</a> is not set to `COPY`. See [MDEV-17468](https://jira.mariadb.org/browse/MDEV-17468) for more information.
</li><li>It also does <strong>not</strong> support altering an existing column into a `VIRTUAL` generated column.
</li><li>It supports using generated columns to [partition tables](/mariadb-administration/partitioning-tables/).
</li><li>It does <strong>not</strong> support using the [versioning clauses](/sql-statements-structure/temporal-tables/system-versioned-tables/) with generated columns.
</li></ul>

- The [SHOW CREATE TABLE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-table/) statement supports generated columns.

- The [DESCRIBE](/sql-statements-structure/sql-statements/administrative-sql-statements/describe/) statement can be used to check whether a table has generated columns.
<ul start="1"><li>You can tell which columns are generated by looking for the ones where the `Extra` column is set to either `VIRTUAL` or `PERSISTENT`. For example:
</li></ul>

```sql
DESCRIBE table1;
+-------+-------------+------+-----+---------+------------+
| Field | Type        | Null | Key | Default | Extra      |
+-------+-------------+------+-----+---------+------------+
| a     | int(11)     | NO   |     | NULL    |            |
| b     | varchar(32) | YES  |     | NULL    |            |
| c     | int(11)     | YES  |     | NULL    | VIRTUAL    |
| d     | varchar(5)  | YES  |     | NULL    | PERSISTENT |
+-------+-------------+------+-----+---------+------------+
```

- Generated columns can be properly referenced in the `NEW` and `OLD` rows in [triggers](/programming-customizing-mariadb/triggers-events/triggers/).

- [Stored procedures](/programming-customizing-mariadb/stored-routines/stored-procedures/) support generated columns.

- The [HANDLER](/sql-statements-structure/nosql/handler/handler-commands/) statement supports generated columns.

### Expression Support

- Most legal, deterministic expressions which can be calculated are supported in expressions for generated columns.

- Most [built-in functions](/built-in-functions/) are supported in expressions for generated columns.
<ul start="1"><li>However, some [built-in functions](/built-in-functions/) can't be supported for technical reasons. For example,  If you try to use an unsupported function in an expression, an error is generated similar to the following:
</li></ul>

```sql
ERROR 1901 (HY000): Function or expression 'dayname()' cannot be used in the GENERATED ALWAYS AS clause of `v`
```

- [Subqueries](/kb/en/subqueries/) are <strong>not</strong> supported in expressions for generated columns because the underlying data can change.

- Using anything that depends on data outside the row is <strong>not</strong> supported in expressions for generated columns.

- [Stored functions](/programming-customizing-mariadb/stored-routines/stored-functions/) are <strong>not</strong> supported in expressions for generated columns. See [MDEV-17587](https://jira.mariadb.org/browse/MDEV-17587) for more information.

##### MariaDB starting with [10.2.1](/kb/en/mariadb-1021-release-notes/)

In [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/) and later, the following statements apply to expressions for generated columns:

- Non-deterministic [built-in functions](/built-in-functions/) are supported in expressions for not indexed `VIRTUAL` generated columns.

- Non-deterministic [built-in functions](/built-in-functions/) are <strong>not</strong> supported in expressions for `PERSISTENT` or indexed `VIRTUAL` generated columns.

- [User-defined functions (UDFs)](/programming-customizing-mariadb/user-defined-functions/) are supported in expressions for generated columns.
<ul start="1"><li>However, MariaDB can't check whether a UDF is deterministic, so it is up to the user to be sure that they do not use non-deterministic UDFs with `VIRTUAL` generated columns.
</li></ul>

- Defining a generated column based on other generated columns defined before it in the table definition is supported. For example:

```sql
CREATE TABLE t1 (a int as (1), b int as (a));
```

- However, defining a generated column based on other generated columns defined after in the table definition is <strong>not</strong> supported in expressions for generation columns because generated columns are calculated in the order they are defined.

- Using an expression that exceeds 255 characters in length is supported in expressions for generated columns. The new limit for the entire table definition, including all expressions for generated columns, is 65,535 bytes.

- Using constant expressions is supported in expressions for generated columns. For example:

```sql
CREATE TABLE t1 (a int as (1));
```

##### MariaDB until [10.2.0](/kb/en/mariadb-1020-release-notes/)

In [MariaDB 10.2.0](/kb/en/mariadb-1020-release-notes/) and before, the following statements apply to expressions for generated columns:

- Non-deterministic [built-in functions](/built-in-functions/) are <strong>not</strong> supported in expressions for generated columns.

- [User-defined functions (UDFs)](/programming-customizing-mariadb/user-defined-functions/) are <strong>not</strong> supported in expressions for generated columns.

- Defining a generated column based on other generated columns defined in the table is <strong>not</strong> supported. Otherwise, it would generate errors like this:

```sql
ERROR 1900 (HY000): A computed column cannot be based on a computed column
```

- Using an expression that exceeds 255 characters in length is <strong>not</strong> supported in expressions for generated columns.

- Using constant expressions is <strong>not</strong> supported in expressions for generated columns. Otherwise, it would generate errors like this:

```sql
ERROR 1908 (HY000): Constant expression in computed column function is not allowed
```

### Making Stored Values Consistent

When a generated column is `PERSISTENT` or indexed, the value of the expression needs to be consistent regardless of the [SQL Mode](/mariadb-administration/variables-and-modes/sql-mode/) flags in the current session.  If it is not, then the table will be seen as corrupted when the value that should actually be returned by the computed expression and the value that was previously stored and/or indexed using a different [sql_mode](/mariadb-administration/variables-and-modes/sql-mode/) setting disagree.

There are currently two affected classes of inconsistencies: character padding and unsigned subtraction:

- For a `VARCHAR` or `TEXT` generated column the length of the value returned can vary depending on the PAD_CHAR_TO_FULL_LENGTH [sql_mode](/mariadb-administration/variables-and-modes/sql-mode/) flag.  To make the value consistent, create the generated column using an RTRIM() or RPAD() function.  Alternately, create the generated column as a `CHAR` column so that its data is always fully padded.

- If a `SIGNED` generated column is based on the subtraction of an `UNSIGNED` value, the resulting value can vary depending on how large the value is and the NO_UNSIGNED_SUBTRACTION [sql_mode](/mariadb-administration/variables-and-modes/sql-mode/) flag.  To make the value consistent, use [CAST()](/built-in-functions/string-functions/cast/) to ensure that each `UNSIGNED` operand is `SIGNED` before the subtraction.

##### MariaDB starting with [10.5](/kb/en/what-is-mariadb-105/)

Beginning in [MariaDB 10.5](/kb/en/what-is-mariadb-105/), there is a fatal error generated when trying to create a generated column whose value can change depending on the [SQL Mode](/mariadb-administration/variables-and-modes/sql-mode/) when its data is `PERSISTENT` or indexed.

For an existing generated column that has a potentially inconsistent value, a warning about a bad expression is generated the first time it is used (if warnings are enabled).

Beginning in [MariaDB 10.4.8](/kb/en/mariadb-1048-release-notes/), [MariaDB 10.3.18](/kb/en/mariadb-10318-release-notes/), and [MariaDB 10.2.27](/kb/en/mariadb-10227-release-notes/) a potentially inconsistent generated column outputs a warning when created or first used (without restricting their creation).

Here is an example of two tables that would be rejected in [MariaDB 10.5](/kb/en/what-is-mariadb-105/) and warned about in the other listed versions:

```sql
CREATE TABLE bad_pad (
  txt CHAR(5),
  -- CHAR -> VARCHAR or CHAR -> TEXT can't be persistent or indexed:
  vtxt VARCHAR(5) AS (txt) PERSISTENT
);

CREATE TABLE bad_sub (
  num1 BIGINT UNSIGNED,
  num2 BIGINT UNSIGNED,
  -- The resulting value can vary for some large values
  vnum BIGINT AS (num1 - num2) VIRTUAL,
  KEY(vnum)
);
```

The warnings for the above tables look like this:

```sql
Warning (Code 1901): Function or expression '`txt`' cannot be used in the GENERATED ALWAYS AS clause of `vtxt`
Warning (Code 1105): Expression depends on the @@sql_mode value PAD_CHAR_TO_FULL_LENGTH

Warning (Code 1901): Function or expression '`num1` - `num2`' cannot be used in the GENERATED ALWAYS AS clause of `vnum`
Warning (Code 1105): Expression depends on the @@sql_mode value NO_UNSIGNED_SUBTRACTION
```

To work around the issue, force the padding or type to make the generated column's expression return a consistent value.  For example:

```sql
CREATE TABLE good_pad (
  txt CHAR(5),
  -- Using RTRIM() or RPAD() makes the value consistent:
  vtxt VARCHAR(5) AS (RTRIM(txt)) PERSISTENT,
  -- When not persistent or indexed, it is OK for the value to vary by mode:
  vtxt2 VARCHAR(5) AS (txt) VIRTUAL,
  -- CHAR -> CHAR is always OK:
  txt2 CHAR(5) AS (txt) PERSISTENT
);

CREATE TABLE good_sub (
  num1 BIGINT UNSIGNED,
  num2 BIGINT UNSIGNED,
  -- The indexed value will always be consistent in this expression:
  vnum BIGINT AS (CAST(num1 AS SIGNED) - CAST(num2 AS SIGNED)) VIRTUAL,
  KEY(vnum)
);
```

### MySQL Compatibility Support

##### MariaDB starting with [10.2.1](/kb/en/mariadb-1021-release-notes/)

In [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/) and later, the following statements apply to MySQL compatibility for generated columns:

- The `STORED` keyword is supported as an alias for the `PERSISTENT` keyword.

- Tables created with MySQL 5.7 or later that contain [MySQL's generated columns](https://dev.mysql.com/doc/refman/5.7/en/create-table-generated-columns.html) can be imported into MariaDB without a dump and restore.

##### MariaDB until [10.2.0](/kb/en/mariadb-1020-release-notes/)

In [MariaDB 10.2.0](/kb/en/mariadb-1020-release-notes/) and before, the following statements apply to MySQL compatibility for generated columns:

- The `STORED` keyword is <strong>not</strong> supported as an alias for the `PERSISTENT` keyword.

- Tables created with MySQL 5.7 or later that contain [MySQL's generated columns](https://dev.mysql.com/doc/refman/5.7/en/create-table-generated-columns.html) can <strong>not</strong> be imported into MariaDB without a dump and restore.

## Implementation Differences

Generated columns are subject to various constraints in other DBMSs that are not present in MariaDB's implementation. Generated columns may also be called computed columns or virtual columns in different implementations. The various details for a specific implementation can be found in the documentation for each specific DBMS.

### Implementation Differences Compared to Microsoft SQL Server

MariaDB's generated columns implementation does not enforce the following
restrictions that are present in [Microsoft SQL Server's computed columns](https://docs.microsoft.com/en-us/sql/relational-databases/tables/specify-computed-columns-in-a-table?view=sql-server-2017) implementation:

- MariaDB allows [server variables](/replication/optimization-and-tuning/system-variables/) in generated column expressions, including those that change dynamically, such as <a undefined>warning_count</a>.
- MariaDB allows the [CONVERT_TZ()](/built-in-functions/date-time-functions/convert_tz/) function to be called with a named [time zone](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/time-zones/) as an argument, even though time zone names and time offsets are configurable.
- MariaDB allows the [CAST()](/built-in-functions/string-functions/cast/) function to be used with non-unicode [character sets](/kb/en/data-types-character-sets-and-collations/), even though character sets are configurable and differ between binaries/versions.
- MariaDB allows [FLOAT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/float/) expressions to be used in generated columns. Microsoft SQL Server considers these expressions to be "imprecise" due to potential cross-platform differences in floating-point implementations and precision.
- Microsoft SQL Server requires the <a undefined>ARITHABORT</a> mode to be set, so that division by zero returns an error, and not a NULL.
- Microsoft SQL Server requires `QUOTED_IDENTIFIER` to be set in [sql_mode](/mariadb-administration/variables-and-modes/sql-mode/). In MariaDB, if data is inserted without `ANSI_QUOTES` set in [sql_mode](/mariadb-administration/variables-and-modes/sql-mode/), then it will be processed and stored differently in a generated column that contains quoted identifiers.
- In [MariaDB 10.2.0](/kb/en/mariadb-1020-release-notes/) and before, it does not allow [user-defined functions (UDFs)](/programming-customizing-mariadb/user-defined-functions/) to be used in expressions for generated columns.

Microsoft SQL Server enforces the above restrictions by doing one of the following things:

- Refusing to create computed columns.
- Refusing to allow updates to a table containing them.
- Refusing to use an index over such a column if it can not be guaranteed that the expression is fully deterministic.

In MariaDB, as long as the [sql_mode](/mariadb-administration/variables-and-modes/sql-mode/), language, and other settings that were in effect during the CREATE TABLE remain unchanged, the generated column expression will always be evaluated the same. If any of these things change, then please be aware that the generated column expression might not be
evaluated the same way as it previously was.

In [MariaDB 5.2](/kb/en/what-is-mariadb-52/), you will get a warning if you try to update a virtual column. In [MariaDB 5.3](/kb/en/what-is-mariadb-53/) and later, this warning will be converted to an error if [strict mode](/kb/en/sql-mode/#strict-mode) is enabled in [sql_mode](/mariadb-administration/variables-and-modes/sql-mode/).

## Development History

Generated columns was originally developed by Andrey Zhakov. It was then modified by Sanja Byelkin and Igor Babaev at Monty Program for inclusion in MariaDB. Monty did the work on [MariaDB 10.2](/kb/en/what-is-mariadb-102/) to lift a some of the old limitations.

## Examples

Here is an example table that uses both <code class="fixed" style="white-space:pre-wrap">VIRTUAL</code> and
<code class="fixed" style="white-space:pre-wrap">PERSISTENT</code> virtual columns:

```sql
USE TEST;

CREATE TABLE table1 (
     a INT NOT NULL,
     b VARCHAR(32),
     c INT AS (a mod 10) VIRTUAL,
     d VARCHAR(5) AS (left(b,5)) PERSISTENT);
```

If you describe the table, you can easily see which columns are virtual by
looking in the "Extra" column:

```sql
DESCRIBE table1;
+-------+-------------+------+-----+---------+------------+
| Field | Type        | Null | Key | Default | Extra      |
+-------+-------------+------+-----+---------+------------+
| a     | int(11)     | NO   |     | NULL    |            |
| b     | varchar(32) | YES  |     | NULL    |            |
| c     | int(11)     | YES  |     | NULL    | VIRTUAL    |
| d     | varchar(5)  | YES  |     | NULL    | PERSISTENT |
+-------+-------------+------+-----+---------+------------+
```

To find out what function(s) generate the value of the virtual column you can use <code class="fixed" style="white-space:pre-wrap">SHOW CREATE TABLE</code>:

```sql
SHOW CREATE TABLE table1;

| table1 | CREATE TABLE `table1` (
  `a` int(11) NOT NULL,
  `b` varchar(32) DEFAULT NULL,
  `c` int(11) AS (a mod 10) VIRTUAL,
  `d` varchar(5) AS (left(b,5)) PERSISTENT
) ENGINE=MyISAM DEFAULT CHARSET=latin1 |
```

If you try to insert non-default values into a virtual column, you will receive
a warning and what you tried to insert will be ignored and the derived value
inserted instead:

```sql
WARNINGS;
Show warnings enabled.

INSERT INTO table1 VALUES (1, 'some text',default,default);
Query OK, 1 row affected (0.00 sec)

INSERT INTO table1 VALUES (2, 'more text',5,default);
Query OK, 1 row affected, 1 warning (0.00 sec)

Warning (Code 1645): The value specified for computed column 'c' in table 'table1' has been ignored.

INSERT INTO table1 VALUES (123, 'even more text',default,'something');
Query OK, 1 row affected, 2 warnings (0.00 sec)

Warning (Code 1645): The value specified for computed column 'd' in table 'table1' has been ignored.
Warning (Code 1265): Data truncated for column 'd' at row 1

SELECT * FROM table1;
+-----+----------------+------+-------+
| a   | b              | c    | d     |
+-----+----------------+------+-------+
|   1 | some text      |    1 | some  |
|   2 | more text      |    2 | more  |
| 123 | even more text |    3 | even  |
+-----+----------------+------+-------+
3 rows in set (0.00 sec)
```

If the `ZEROFILL` clause is specified, it should be placed directly after the type definition, before the `AS (&lt;expression&gt;)`:

```sql
CREATE TABLE table2 (a INT, b INT ZEROFILL AS (a*2) VIRTUAL);
INSERT INTO table2 (a) VALUES (1);

SELECT * FROM table2;
+------+------------+
| a    | b          |
+------+------------+
|    1 | 0000000002 |
+------+------------+
1 row in set (0.00 sec)
```

You can also use virtual columns to implement a "poor man's partial index". See example at the end of [Unique Index](/kb/en/getting-started-with-indexes/#unique-index).

## See Also

- [Putting Virtual Columns to good use](https://mariadb.com/blog/putting-virtual-columns-good-use) on the mariadb.com blog.