# Dynamic Columns from MariaDB 10

##### MariaDB starting with [10.0.1](/kb/en/mariadb-1001-release-notes/)

[MariaDB 10.0.1](/kb/en/mariadb-1001-release-notes/) introduced the following improvements to the [dynamic columns](/sql-statements-structure/nosql/dynamic-columns/) feature.

## Column Name Support

It is possible to refer to column by names. Names can be used everywhere where in [MariaDB 5.3](/kb/en/what-is-mariadb-53/) one could use only strings:

- Create a dynamic column blob:

```sql
COLUMN_CREATE('int_col', 123 as int, 'double_col', 3.14 as double, 'string_col', 'text-data' as char);
```

- Set a column value:

```sql
COLUMN_ADD(dyncol_blob, 'intcol', 1234);
```

- Get a column value:

```sql
COLUMN_GET(dynstr, 'column1' as char(10));
```

- Check whether a column exists

```sql
COLUMN_EXISTS(dyncol_blob, 'column_name');
```

## Changes in Behavior

- Column list output now includes quoting:

```sql
select column_list(column_create(1, 22, 2, 23));
+------------------------------------------+
| column_list(column_create(1, 22, 2, 23)) |
+------------------------------------------+
| `1`,`2`                                  |
+------------------------------------------+
select column_list(column_create('column1', 22, 'column2', 23)); 
+----------------------------------------------------------+
| column_list(column_create('column1', 22, 'column2', 23)) |
+----------------------------------------------------------+
| `column1`,`column2`                                      |
+----------------------------------------------------------+
```

- Column name interpretation has been changed so that the string now is not converted to a number. So some "magic" tricks will not work any more, for example, "1test" and "1" now become different column names:

```sql
select column_list(column_add(column_create('1a', 22), '1b', 23));
+------------------------------------------------------------+
| column_list(column_add(column_create('1a', 22), '1b', 23)) |
+------------------------------------------------------------+
| `1a`,`1b`                                                  |
+------------------------------------------------------------+
```

- Old behavior:

```sql
select column_list(column_add(column_create('1a', 22), '1b', 23));
+------------------------------------------------------------+
| column_list(column_add(column_create('1a', 22), '1b', 23)) |
+------------------------------------------------------------+
| 1                                                          |
+------------------------------------------------------------+
```

## New Functions

The following new functions have been added to dynamic columns in MariaDB 10

### COLUMN_CHECK

[COLUMN_CHECK](/built-in-functions/special-functions/dynamic-columns-functions/column_check/) is used to check a column's integrity. When it encounters an error
it does not return illegal format errors but returns false instead. It also
checks integrity more thoroughly and finds errors in the dynamic column
internal structures which might not be found by other functions.

```sql
select column_check(column_create('column1', 22));
+--------------------------------------------+
| column_check(column_create('column1', 22)) |
+--------------------------------------------+
|                                          1 |
+--------------------------------------------+
select column_check('abracadabra');
+-----------------------------+
| column_check('abracadabra') |
+-----------------------------+
|                           0 |
+-----------------------------+
```

### COLUMN_JSON

[COLUMN_JSON](column-json) converts all dynamic column record content to a JSON object.

```sql
select column_json(column_create('column1', 1, 'column2', "two"));
+------------------------------------------------------------+
| column_json(column_create('column1', 1, 'column2', "two")) |
+------------------------------------------------------------+
| {"column1":1,"column2":"two"}                              |
+------------------------------------------------------------+
```

## Other Changes

- All API functions has prefix mariadb_dyncol_ (old prefix dynamic_column_ is depricated
- API changed to be able to work with the new format (*_named functions).
- Removed 'delete' function because deleting could be done by adding NULL value.
- 'Time' and 'datetime' in the new format are stored without microseconds if they are 0.
- New function added to API (except that two which are representing SQL level functions):
<ul start="1"><li>'Unpack' the dynamic columns content to an arrays of values and names.
</li><li>3 functions to get any column value as string, integer (long long) or floating point (double).
</li></ul>
- New type of "dynamic column" row added on the API level (in SQL level output it is a string but if you use dynamic column functions to construct object it will be added as dynamic column value) which allow to add dynamic columns inside dynamic columns. JSON function represent such recursive constructions correctly but limit depth of representation as current implementation limit (internally depth of dynamic columns embedding is not limited).

## Interface with Cassandra

CassandraSE is no longer actively being developed and has been removed in [MariaDB 10.6](/kb/en/what-is-mariadb-106/). See [MDEV-23024](https://jira.mariadb.org/browse/MDEV-23024).

Some internal changes were added to dynamic columns to allow them to serve as
an interface to Apache Cassandra dynamic columns. The [Cassandra engine](/kb/en/cassandra-storage-engine/) may pack all columns which were not mentioned in the MariaDB interface table definition
and even bring changes in the dynamic column contents back to the cassandra
columns family (the table analog in cassandra).

## See Also

- [Dynamic Columns](/sql-statements-structure/nosql/dynamic-columns/)
- [Cassandra Storage Engine](/kb/en/cassandra-storage-engine/)