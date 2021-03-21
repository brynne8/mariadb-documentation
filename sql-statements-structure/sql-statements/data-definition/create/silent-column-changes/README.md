# Silent Column Changes

When a [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table) or [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table) command is issued, MariaDB will silently change a column specification in the following cases:

- [PRIMARY KEY](/kb/en/getting-started-with-indexes/#primary-key) columns are always NOT NULL.
- Any trailing spaces from [SET](/columns-storage-engines-and-plugins/data-types/string-data-types/set-data-type) and [ENUM](/columns-storage-engines-and-plugins/data-types/string-data-types/enum) values are discarded.
- [TIMESTAMP](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/timestamp) columns are always NOT NULL, and display sizes are discarded
- A row-size limit of 65535 bytes applies
- If strict SQL mode is not enabled, a [VARCHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/varchar) column longer than 65535 become [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text), and a [VARBINARY](/columns-storage-engines-and-plugins/data-types/string-data-types/varbinary) columns longer than 65535 becomes a [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob). If strict mode is enabled the silent changes will not be made, and an error will occur.
- If a USING clause specifies an index that's not permitted by the storage engine, the engine will instead use another available index type that can be applied without affecting results.
- If the CHARACTER SET binary attribute is specified, the column is created as the matching binary data type. A TEXT becomes a BLOB, CHAR a BINARY and VARCHAR a VARBINARY. ENUMs and SETs are created as defined.

To ease imports from other RDBMS's, MariaDB will also silently map the following data types:

<table><tbody><tr><th>Other Vendor Type</th><th>MariaDB Type</th></tr>
<tr><td>BOOL</td><td><a href="/kb/en/tinyint/">TINYINT</a></td></tr>
<tr><td>BOOLEAN</td><td><a href="/kb/en/tinyint/">TINYINT</a></td></tr>
<tr><td>CHARACTER VARYING(M)</td><td><a href="/kb/en/varchar/">VARCHAR</a>(M)</td></tr>
<tr><td>FIXED</td><td><a href="/kb/en/decimal/">DECIMAL</a></td></tr>
<tr><td>FLOAT4</td><td><a href="/kb/en/float/">FLOAT</a></td></tr>
<tr><td>FLOAT8</td><td><a href="/kb/en/double/">DOUBLE</a></td></tr>
<tr><td>INT1</td><td><a href="/kb/en/tinyint/">TINYINT</a></td></tr>
<tr><td>INT2</td><td><a href="/kb/en/smallint/">SMALLINT</a></td></tr>
<tr><td>INT3</td><td><a href="/kb/en/mediumint/">MEDIUMINT</a></td></tr>
<tr><td>INT4</td><td><a href="/kb/en/int/">INT</a></td></tr>
<tr><td>INT8</td><td><a href="/kb/en/bigint/">BIGINT</a></td></tr>
<tr><td>LONG VARBINARY</td><td><a href="/kb/en/mediumblob/">MEDIUMBLOB</a></td></tr>
<tr><td>LONG VARCHAR</td><td><a href="/kb/en/mediumtext/">MEDIUMTEXT</a></td></tr>
<tr><td>LONG</td><td><a href="/kb/en/mediumtext/">MEDIUMTEXT</a></td></tr>
<tr><td>MIDDLEINT</td><td><a href="/kb/en/mediumint/">MEDIUMINT</a></td></tr>
<tr><td>NUMERIC</td><td><a href="/kb/en/decimal/">DECIMAL</a></td></tr>
</tbody></table>

Currently, all MySQL types are supported in MariaDB.

For type mapping between Cassandra and MariaDB, see [Cassandra storage engine](/kb/en/cassandra-storage-engine/#datatypes).

## Example

Silent changes in action:

```sql
CREATE TABLE SilenceIsGolden
   (
    f1 TEXT CHARACTER SET binary,
    f2 VARCHAR(15) CHARACTER SET binary,
    f3 CHAR CHARACTER SET binary,
    f4 ENUM('x','y','z') CHARACTER SET binary,
    f5 VARCHAR (65536),
    f6 VARBINARY (65536),
    f7 INT1
   );
Query OK, 0 rows affected, 2 warnings (0.31 sec)

SHOW WARNINGS;
+-------+------+-----------------------------------------------+
| Level | Code | Message                                       |
+-------+------+-----------------------------------------------+
| Note  | 1246 | Converting column 'f5' from VARCHAR to TEXT   |
| Note  | 1246 | Converting column 'f6' from VARBINARY to BLOB |
+-------+------+-----------------------------------------------+

DESCRIBE SilenceIsGolden;
+-------+-------------------+------+-----+---------+-------+
| Field | Type              | Null | Key | Default | Extra |
+-------+-------------------+------+-----+---------+-------+
| f1    | blob              | YES  |     | NULL    |       |
| f2    | varbinary(15)     | YES  |     | NULL    |       |
| f3    | binary(1)         | YES  |     | NULL    |       |
| f4    | enum('x','y','z') | YES  |     | NULL    |       |
| f5    | mediumtext        | YES  |     | NULL    |       |
| f6    | mediumblob        | YES  |     | NULL    |       |
| f7    | tinyint(4)        | YES  |     | NULL    |       |
+-------+-------------------+------+-----+---------+-------+
```