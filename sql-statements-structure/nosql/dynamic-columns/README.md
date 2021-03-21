# Dynamic Columns

Dynamic columns allow one to store different sets of columns for each row in a
table. It works by storing a set of columns in a blob and having a small set of
functions to manipulate it.

Dynamic columns should be used when it is not possible to use regular columns.

A typical use case is when one needs to store items that may have many
different attributes (like size, color, weight, etc), and the set of possible
attributes is very large and/or unknown in advance. In that case, attributes
can be put into dynamic columns.

## Dynamic Columns Basics

The table should have a blob column which will be used as storage for dynamic
columns:

```sql
create table assets (
  item_name varchar(32) primary key, -- A common attribute for all items
  dynamic_cols  blob  -- Dynamic columns will be stored here
);
```

Once created, one can access dynamic columns via dynamic column functions:

Insert a row with two dynamic columns: color=blue, size=XL

```sql
INSERT INTO assets VALUES 
  ('MariaDB T-shirt', COLUMN_CREATE('color', 'blue', 'size', 'XL'));
```

Insert another row with dynamic columns: color=black, price=500

```sql
INSERT INTO assets VALUES
  ('Thinkpad Laptop', COLUMN_CREATE('color', 'black', 'price', 500));
```

Select dynamic column 'color' for all items:

```sql
SELECT item_name, COLUMN_GET(dynamic_cols, 'color' as char) AS color FROM assets;
+-----------------+-------+
| item_name       | color |
+-----------------+-------+
| MariaDB T-shirt | blue  |
| Thinkpad Laptop | black |
+-----------------+-------+
```

<em>(note: the above example uses [MariaDB 10.0.1](/kb/en/mariadb-1001-release-notes/). In [MariaDB 5.3](/kb/en/what-is-mariadb-53/), columns can only be identified by numbers. See the [#mariadb-53-vs-mariadb-100](#mariadb-53-vs-mariadb-100) section below)</em>

It is possible to add and remove dynamic columns from a row:

```sql
-- Remove a column:
UPDATE assets SET dynamic_cols=COLUMN_DELETE(dynamic_cols, "price") 
WHERE COLUMN_GET(dynamic_cols, 'color' as char)='black'; 

-- Add a column:
UPDATE assets SET dynamic_cols=COLUMN_ADD(dynamic_cols, 'warranty', '3 years')
WHERE item_name='Thinkpad Laptop';
```

You can also list all columns, or (starting from [MariaDB 10.0.1](/kb/en/mariadb-1001-release-notes/)) get them
together with their values in JSON format:

```sql
SELECT item_name, column_list(dynamic_cols) FROM assets;
+-----------------+---------------------------+
| item_name       | column_list(dynamic_cols) |
+-----------------+---------------------------+
| MariaDB T-shirt | `size`,`color`            |
| Thinkpad Laptop | `color`,`warranty`        |
+-----------------+---------------------------+

SELECT item_name, COLUMN_JSON(dynamic_cols) FROM assets;
+-----------------+----------------------------------------+
| item_name       | COLUMN_JSON(dynamic_cols)              |
+-----------------+----------------------------------------+
| MariaDB T-shirt | {"size":"XL","color":"blue"}           |
| Thinkpad Laptop | {"color":"black","warranty":"3 years"} |
+-----------------+----------------------------------------+
```

## Dynamic Columns Reference

The rest of this page is a complete reference of dynamic columns in MariaDB

### Dynamic Columns Functions

#### COLUMN_CREATE

```sql
COLUMN_CREATE(column_nr, value [as type], [column_nr, value [as type]]...);
COLUMN_CREATE(column_name, value [as type], [column_name, value [as type]]...);
```

Return a dynamic columns blob that stores the specified columns with values.

The return value is suitable for

- <ul start="1"><li>storing in a table
</li><li>further modification with other dynamic columns functions
</li></ul>

The <strong>`as type`</strong> part allows one to specify the value type. In most cases,
 this is redundant because MariaDB will be able to deduce the type of the
 value. Explicit type specification may be needed when the type of the value is
 not apparent. For example, a literal `'2012-12-01'` has a CHAR type by
 default, one will need to specify `'2012-12-01' AS DATE` to have it stored as
 a date. See the [Datatypes](#Datatypes) section for further details. Note also [MDEV-597](https://jira.mariadb.org/browse/MDEV-597).

Typical usage:

```sql
-- MariaDB 5.3+:
INSERT INTO tbl SET dyncol_blob=COLUMN_CREATE(1 /*column id*/, "value");
-- MariaDB 10.0.1+:
INSERT INTO tbl SET dyncol_blob=COLUMN_CREATE("column_name", "value");
```

#### COLUMN_ADD

```sql
COLUMN_ADD(dyncol_blob, column_nr, value [as type], [column_nr, value [as type]]...);
COLUMN_ADD(dyncol_blob, column_name, value [as type], [column_name, value [as type]]...);
```

Adds or updates dynamic columns.

- <ul start="1"><li><strong>`dyncol_blob`</strong> must be either a valid dynamic columns blob (for example, `COLUMN_CREATE` returns such blob), or an empty string.
</li><li><strong>`column_name`</strong> specifies the name of the column to be added. If `dyncol_blob` already has a column with this name, it will be overwritten.
</li><li><strong>`value`</strong> specifies the new value for the column.  Passing a NULL value will cause the column to be deleted.
</li><li><strong>`as type`</strong> is optional. See [#datatypes](#datatypes) section for a discussion about types.
</li></ul>

The return value is a dynamic column blob after the modifications.

Typical usage:

```sql
-- MariaDB 5.3+:
UPDATE tbl SET dyncol_blob=COLUMN_ADD(dyncol_blob, 1 /*column id*/, "value") WHERE id=1;
-- MariaDB 10.0.1+:
UPDATE t1 SET dyncol_blob=COLUMN_ADD(dyncol_blob, "column_name", "value") WHERE id=1;
```

Note: `COLUMN_ADD()` is a regular function (just like
 [CONCAT()](/built-in-functions/string-functions/concat)), hence, in order to update the value in the table
 you have to use the <code>UPDATE ... SET dynamic_col=COLUMN_ADD(dynamic_col,
 ....) </code> pattern.

#### COLUMN_GET

```sql
COLUMN_GET(dyncol_blob, column_nr as type);
COLUMN_GET(dyncol_blob, column_name as type);
```

Get the value of a dynamic column by its name. If no column with the given
 name exists, `NULL` will be returned.

<strong>`column_name as type`</strong> requires that one specify the datatype of the
 dynamic column they are reading.

This may seem counter-intuitive: why would one need to specify which datatype
 they're retrieving? Can't the dynamic columns system figure the datatype from
 the data being stored?

The answer is: SQL is a statically-typed language. The SQL interpreter needs
 to know the datatypes of all expressions before the query is run (for
 example, when one is using prepared statements and runs
 `"select COLUMN_GET(...)"`, the prepared statement API requires the server
 to inform the client about the datatype of the column being read before the
 query is executed and the server can see what datatype the column actually
 has).

See the [Datatypes](#datatypes) section for more information about
 datatypes.

#### COLUMN_DELETE

```sql
COLUMN_DELETE(dyncol_blob, column_nr, column_nr...);
COLUMN_DELETE(dyncol_blob, column_name, column_name...);
```

Delete a dynamic column with the specified name. Multiple names can be given.

The return value is a dynamic column blob after the modification.

#### COLUMN_EXISTS

```sql
COLUMN_EXISTS(dyncol_blob, column_nr);
COLUMN_EXISTS(dyncol_blob, column_name);
```

Check if a column with name `column_name` exists in `dyncol_blob`. If
 yes, return `1`, otherwise return `0`.

#### COLUMN_LIST

```sql
COLUMN_LIST(dyncol_blob);
```

Before [MariaDB 10.0.1](/kb/en/mariadb-1001-release-notes/): Return a comma-separated list of column numbers.
 After [MariaDB 10.0.1](/kb/en/mariadb-1001-release-notes/): Return a comma-separated list of column names. The
 names are quoted with backticks.

Example using [MariaDB 10.0.1](/kb/en/mariadb-1001-release-notes/):

```sql
SELECT column_list(column_create('col1','val1','col2','val2'));
+---------------------------------------------------------+
| column_list(column_create('col1','val1','col2','val2')) |
+---------------------------------------------------------+
| `col1`,`col2`                                           |
+---------------------------------------------------------+
```

##### MariaDB starting with [10.0.1](/kb/en/mariadb-1001-release-notes/)

#### COLUMN_CHECK

```sql
COLUMN_CHECK(dyncol_blob);
```

Check if `dyncol_blob` is a valid packed dynamic columns blob. Return value
 of 1 means the blob is valid, return value of 0 means it is not.

<strong>Rationale:</strong>
 Normally, one works with valid dynamic column blobs. Functions like
 `COLUMN_CREATE`, `COLUMN_ADD`, `COLUMN_DELETE` always return valid
 dynamic column blobs. However, if a dynamic column blob is accidentally
 truncated, or transcoded from one character set to another, it will be
 corrupted. This function can be used to check if a value in a blob field is a
 valid dynamic column blob.

<strong>Note:</strong> 
 It is possible that a truncation cut a Dynamic Column "clearly" so that COLUMN_CHECK will not notice the corruption, but in any case of truncation a warning is issued during value storing.

<em>This function was introduced in [MariaDB 10.0.1](/kb/en/mariadb-1001-release-notes/).</em>

#### COLUMN_JSON

```sql
COLUMN_JSON(dyncol_blob);
```

Return a JSON representation of data in `dyncol_blob`.

Example:

```sql
SELECT item_name, COLUMN_JSON(dynamic_cols) FROM assets;
+-----------------+----------------------------------------+
| item_name       | COLUMN_JSON(dynamic_cols)              |
+-----------------+----------------------------------------+
| MariaDB T-shirt | {"size":"XL","color":"blue"}           |
| Thinkpad Laptop | {"color":"black","warranty":"3 years"} |
+-----------------+----------------------------------------+
```

Limitation: `COLUMN_JSON` will decode nested dynamic columns at a nesting
 level of not more than 10 levels deep. Dynamic columns that are nested deeper
 than 10 levels will be shown as BINARY string, without encoding.

<em>This function was introduced in [MariaDB 10.0.1](/kb/en/mariadb-1001-release-notes/).</em>

### Nesting Dynamic Columns

It is possible to use nested dynamic columns by putting one dynamic column blob
inside another. The `COLUMN_JSON` function will display nested columns.

```sql
SET @tmp= column_create('parent_column', column_create('child_column', 12345));
Query OK, 0 rows affected (0.00 sec)

SELECT column_json(@tmp);
+------------------------------------------+
| column_json(@tmp)                        |
+------------------------------------------+
| {"parent_column":{"child_column":12345}} |
+------------------------------------------+

SELECT column_get(column_get(@tmp, 'parent_column' AS char), 'child_column' AS int);
+------------------------------------------------------------------------------+
| column_get(column_get(@tmp, 'parent_column' as char), 'child_column' as int) |
+------------------------------------------------------------------------------+
|                                                                        12345 |
+------------------------------------------------------------------------------+
```

If you are trying to get a nested dynamic column as a string use 'as BINARY' as the last argument of COLUMN_GET (otherwise problems with character set conversion and illegal symbols are possible):

```sql
select column_json( column_get(
  column_create('test1', column_create('key1','value1','key2','value2','key3','value3')),
  'test1' as BINARY));
```

### Datatypes

In SQL, one needs to define the type of each column in a table. Dynamic columns
do not provide any way to declare a type in advance ("whenever there is a
column 'weight', it should be integer" is not possible).  However, each
particular dynamic column value is stored together with its datatype.

The set of possible datatypes is mostly the same as that used by the SQL
[CAST](/built-in-functions/string-functions/cast) and [CONVERT](/built-in-functions/string-functions/convert) functions. However, note that there are currently some differences - see [MDEV-597](https://jira.mariadb.org/browse/MDEV-597).

<table><tbody><tr><th>type</th><th>dynamic column internal type</th><th>description</th></tr>
<tr><td><code>BINARY[(N)]</code></td><td><code>DYN_COL_STRING</code></td><td>(variable length string with binary charset)</td></tr>
<tr><td><code>CHAR[(N)]</code></td><td><code>DYN_COL_STRING</code></td><td>(variable length string with charset)</td></tr>
<tr><td><code>DATE</code></td><td><code>DYN_COL_DATE</code></td><td>(date - 3 bytes)</td></tr>
<tr><td><code>DATETIME[(D)]</code></td><td><code>DYN_COL_DATETIME</code></td><td>(date and time (with <a href="/kb/en/microseconds-in-mariadb/">microseconds</a>) - 9 bytes)</td></tr>
<tr><td><code>DECIMAL[(M[,D])]</code></td><td><code>DYN_COL_DECIMAL</code></td><td>(variable length binary decimal representation with MariaDB limitation)</td></tr>
<tr><td><code>DOUBLE[(M,D)]</code></td><td><code>DYN_COL_DOUBLE</code></td><td>(64 bit double-precision floating point)</td></tr>
<tr><td><code>INTEGER</code></td><td><code>DYN_COL_INT</code></td><td>(variable length, up to 64 bit signed integer)</td></tr>
<tr><td><code>SIGNED [INTEGER]</code></td><td><code>DYN_COL_INT</code></td><td>(variable length, up to 64 bit signed integer)</td></tr>
<tr><td><code>TIME[(D)]</code></td><td><code>DYN_COL_TIME</code></td><td>(time (with <a href="/kb/en/microseconds-in-mariadb/">microseconds</a>, may be negative) - 6 bytes)</td></tr>
<tr><td><code>UNSIGNED [INTEGER]</code></td><td><code>DYN_COL_UINT</code></td><td>(variable length, up to 64bit unsigned integer)</td></tr>
</tbody></table>

#### A Note About Lengths

If you're running queries like

```sql
SELECT COLUMN_GET(blob, 'colname' as CHAR) ... 
```

without specifying a maximum length (i.e. using #as CHAR#, not `as CHAR(n)`),
MariaDB will report the maximum length of the resultset column to be
`53,6870,911` (bytes or characters?) for [MariaDB 5.3](/kb/en/what-is-mariadb-53/)-10.0.0 and
`16,777,216` for [MariaDB 10.0.1](/kb/en/mariadb-1001-release-notes/)+. This may cause excessive memory usage in
some client libraries, because they try to pre-allocate a buffer of maximum
resultset width. If you suspect you're hitting this problem, use `CHAR(n)`
whenever you're using `COLUMN_GET` in the select list.

### [MariaDB 5.3](/kb/en/what-is-mariadb-53/) vs [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

The dynamic columns feature was introduced into MariaDB in two steps:

1 [MariaDB 5.3](/kb/en/what-is-mariadb-53/) was the first version to support dynamic columns. Only numbers
  could be used as column names in this version.
2 In [MariaDB 10.0.1](/kb/en/mariadb-1001-release-notes/), column names can be either numbers or strings.
  Also, the `COLUMN_JSON` and `COLUMN_CHECK` functions were added.

See also [Dynamic Columns in MariaDB 10](/kb/en/dynamic-columns-in-mariadb-10/).

### Client-side API

It is also possible to create or parse dynamic columns blobs on the client side. `libmysql` client library now includes an API for writing/reading dynamic column blobs.  See [dynamic-columns-api](/sql-statements-structure/nosql/dynamic-columns-api) for details.

### Limitations

<table><tbody><tr><th>Description</th><th>Limit</th></tr>
<tr><td>Max number of columns</td><td>&nbsp;65535</td></tr>
<tr><td>Max total length of packed dynamic column</td><td><a href="/kb/en/server-system-variables/#max_allowed_packet">max_allowed_packet</a> (1G)</td></tr>
</tbody></table>