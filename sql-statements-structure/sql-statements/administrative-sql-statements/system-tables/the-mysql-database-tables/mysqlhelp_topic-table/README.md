# mysql.help_topic Table

`mysql.help_topic` is one of the four tables used by the [HELP command](/sql-statements-structure/sql-statements/administrative-sql-statements/help-command/). It is populated when the server is installed by the `fill_help_table.sql` script. The other help tables are [help_relation](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlhelp_relation-table/), [help_category](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlhelp_category-table/) and [help_keyword](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlhelp_keyword-table/).

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, this table uses the [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/) storage engine.

##### MariaDB until [10.3](/kb/en/what-is-mariadb-103/)

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and before, this table uses the [MyISAM](/columns-storage-engines-and-plugins/storage-engines/myisam-storage-engine/) storage engine.

The `mysql.help_topic` table contains the following fields:

<table><tbody><tr><th>Field</th><th>Type</th><th>Null</th><th>Key</th><th>Default</th><th>Description</th></tr>
<tr><td><code>help_topic_id</code></td><td><code>int(10) unsigned</code></td><td>NO</td><td>PRI</td><td><code>NULL</code></td><td></td></tr>
<tr><td><code>name</code></td><td><code>char(64)</code></td><td>NO</td><td>UNI</td><td><code>NULL</code></td><td></td></tr>
<tr><td><code>help_category_id</code></td><td><code>smallint(5) unsigned</code></td><td>NO</td><td></td><td><code>NULL</code></td><td></td></tr>
<tr><td><code>description</code></td><td><code>text</code></td><td>NO</td><td></td><td><code>NULL</code></td><td></td></tr>
<tr><td><code>example</code></td><td><code>text</code></td><td>NO</td><td></td><td><code>NULL</code></td><td></td></tr>
<tr><td><code>url</code></td><td><code>char(128)</code></td><td>NO</td><td></td><td><code>NULL</code></td><td></td></tr>
</tbody></table>

## Example

```sql
SELECT * FROM help_topic\G;
...
*************************** 508. row ***************************
   help_topic_id: 507
            name: FIND_IN_SET
help_category_id: 37
     description: Syntax:
FIND_IN_SET(str,strlist)

Returns a value in the range of 1 to N if the string str is in the
string list strlist consisting of N substrings. A string list is a
string composed of substrings separated by "," characters. If the first
argument is a constant string and the second is a column of type SET,
the FIND_IN_SET() function is optimized to use bit arithmetic. Returns
0 if str is not in strlist or if strlist is the empty string. Returns
NULL if either argument is NULL. This function does not work properly
if the first argument contains a comma (",") character.

URL: http://dev.mysql.com/doc/refman/5.5/en/string-functions.html


         example: mysql> SELECT FIND_IN_SET('b','a,b,c,d');
        -> 2

             url: http://dev.mysql.com/doc/refman/5.5/en/string-functions.html
```