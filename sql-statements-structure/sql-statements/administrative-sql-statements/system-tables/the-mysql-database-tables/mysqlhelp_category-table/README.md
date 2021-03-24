# mysql.help_category Table

`mysql.help_category` is one of the four tables used by the [HELP command](/sql-statements-structure/sql-statements/administrative-sql-statements/help-command/). It is populated when the server is installed by the `fill_help_table.sql` script. The other help tables are [help_relation](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlhelp_relation-table/), [help_topic](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlhelp_topic-table/) and [help_keyword](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlhelp_keyword-table/).

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, this table uses the [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/) storage engine.

##### MariaDB until [10.3](/kb/en/what-is-mariadb-103/)

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and before, this table uses the [MyISAM](/columns-storage-engines-and-plugins/storage-engines/myisam-storage-engine/) storage engine.

The `mysql.help_category` table contains the following fields:

<table><tbody><tr><th>Field</th><th>Type</th><th>Null</th><th>Key</th><th>Default</th><th>Description</th></tr>
<tr><td><code>help_category_id</code></td><td><code>smallint(5) unsigned</code></td><td>NO</td><td>PRI</td><td><code>NULL</code></td><td></td></tr>
<tr><td><code>name</code></td><td><code>char(64)</code></td><td>NO</td><td>UNI</td><td><code>NULL</code></td><td></td></tr>
<tr><td><code>parent_category_id</code></td><td><code>smallint(5) unsigned</code></td><td>YES</td><td></td><td><code>NULL</code></td><td></td></tr>
<tr><td><code>url</code></td><td><code>char(128)</code></td><td>NO</td><td></td><td><code>NULL</code></td><td></td></tr>
</tbody></table>

## Example

```sql
SELECT * FROM help_category;
+------------------+-----------------------------------------------+--------------------+-----+
| help_category_id | name                                          | parent_category_id | url |
+------------------+-----------------------------------------------+--------------------+-----+
|                1 | Geographic                                    |                  0 |     |
|                2 | Polygon properties                            |                 34 |     |
|                3 | WKT                                           |                 34 |     |
|                4 | Numeric Functions                             |                 38 |     |
|                5 | Plugins                                       |                 35 |     |
|                6 | MBR                                           |                 34 |     |
|                7 | Control flow functions                        |                 38 |     |
|                8 | Transactions                                  |                 35 |     |
|                9 | Help Metadata                                 |                 35 |     |
|               10 | Account Management                            |                 35 |     |
|               11 | Point properties                              |                 34 |     |
|               12 | Encryption Functions                          |                 38 |     |
|               13 | LineString properties                         |                 34 |     |
|               14 | Miscellaneous Functions                       |                 38 |     |
|               15 | Logical operators                             |                 38 |     |
|               16 | Functions and Modifiers for Use with GROUP BY |                 35 |     |
|               17 | Information Functions                         |                 38 |     |
|               18 | Comparison operators                          |                 38 |     |
|               19 | Bit Functions                                 |                 38 |     |
|               20 | Table Maintenance                             |                 35 |     |
|               21 | User-Defined Functions                        |                 35 |     |
|               22 | Data Types                                    |                 35 |     |
|               23 | Compound Statements                           |                 35 |     |
|               24 | Geometry constructors                         |                 34 |     |
|               25 | GeometryCollection properties                 |                  1 |     |
|               26 | Administration                                |                 35 |     |
|               27 | Data Manipulation                             |                 35 |     |
|               28 | Utility                                       |                 35 |     |
|               29 | Language Structure                            |                 35 |     |
|               30 | Geometry relations                            |                 34 |     |
|               31 | Date and Time Functions                       |                 38 |     |
|               32 | WKB                                           |                 34 |     |
|               33 | Procedures                                    |                 35 |     |
|               34 | Geographic Features                           |                 35 |     |
|               35 | Contents                                      |                  0 |     |
|               36 | Geometry properties                           |                 34 |     |
|               37 | String Functions                              |                 38 |     |
|               38 | Functions                                     |                 35 |     |
|               39 | Data Definition                               |                 35 |     |
+------------------+-----------------------------------------------+--------------------+-----+
```