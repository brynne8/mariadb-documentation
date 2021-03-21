# JSON Data Type

##### MariaDB starting with [10.2.7](/kb/en/mariadb-1027-release-notes/)

The JSON alias was added in [MariaDB 10.2.7](/kb/en/mariadb-1027-release-notes/). This was done to make it possible to use JSON columns in [statement based](/kb/en/binary-log-formats/#statement-based) [replication](/kb/en/high-availability-performance-tuning-mariadb-replication/) from MySQL to MariaDB and to make it possible for MariaDB to read [mysqldumps](/clients-utilities/backup-restore-and-import-clients/mysqldump) from MySQL.

JSON is an alias for [LONGTEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/longtext) introduced for compatibility reasons with MySQL's JSON data type. MariaDB implements this as a LONGTEXT rather, as the JSON data type contradicts the SQL standard, and MariaDB's benchmarks indicate that performance is at least equivalent.

In order to ensure that a a valid json document is inserted, the [JSON_VALID](/built-in-functions/special-functions/json-functions/json_valid) function can be used as a [CHECK constraint](/kb/en/constraint/#check-constraint-expressions). This constraint is automatically included for types using the JSON alias from [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/).

## Examples

```sql
CREATE TABLE t (j JSON);

DESC t;
+-------+----------+------+-----+---------+-------+
| Field | Type     | Null | Key | Default | Extra |
+-------+----------+------+-----+---------+-------+
| j     | longtext | YES  |     | NULL    |       |
+-------+----------+------+-----+---------+-------+
```

With validation:

```sql
CREATE TABLE t2 (
  j JSON 
  CHECK (JSON_VALID(j))
);

INSERT INTO t2 VALUES ('invalid');
ERROR 4025 (23000): CONSTRAINT `j` failed for `test`.`t2`

INSERT INTO t2 VALUES ('{"id": 1, "name": "Monty"}');
Query OK, 1 row affected (0.13 sec)
```

## Replicating JSON Data Between MySQL and MariaDB

The JSON type in MySQL stores the JSON object in a compact form, not as [LONGTEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/longtext) as in MariaDB.
This means that row based replication will not work for JSON types from MySQL to MariaDB.

There are a a few different ways to solve this:

- Use statement based replication.
- Change the JSON column to type TEXT in MySQL

## Converting a MySQL TABLE with JSON Fields to MariaDB

MariaDB can't directly access MySQL's JSON format.

There are a a few different ways to move the table to MariaDB:

- Change the JSON column to type TEXT in MySQL.  After this, MariaDB can directly use the table without any need for a dump and restore.
- [Use mysqldump to copy the table](https://mariadb.com/kb/en/library/mysqldump/#examples).

## Differences Between MySQL JSON Strings and MariaDB JSON Strings

- In MySQL, JSON is an object and is [compared according to json values](https://dev.mysql.com/doc/refman/8.0/en/json.html#json-comparison). In MariaDB JSON strings are normal strings and compared as strings. One exception is when using [JSON_EXTRACT()](/built-in-functions/special-functions/json-functions/json_extract) in which case strings are unescaped before comparison.

## See Also

- [JSON Functions](/built-in-functions/special-functions/json-functions)
- [CONNECT JSON Table Type](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-json-table-type)
- [MDEV-9144](https://jira.mariadb.org/browse/MDEV-9144)