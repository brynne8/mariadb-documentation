# SHOW COLUMNS

## Syntax

```sql
SHOW [FULL] {COLUMNS | FIELDS} FROM tbl_name [FROM db_name]
    [LIKE 'pattern' | WHERE expr]
```

## Description

<code class="highlight fixed" style="white-space:pre-wrap">SHOW COLUMNS</code> displays information about the columns in a
given table. It also works for views. The <code class="highlight fixed" style="white-space:pre-wrap">LIKE</code> clause, if
present on its own, indicates which column names to match. The <code class="highlight fixed" style="white-space:pre-wrap">WHERE</code> and <code class="highlight fixed" style="white-space:pre-wrap">LIKE</code> clauses can be given to select rows using more general conditions, as discussed in [Extended SHOW](/sql-statements-structure/sql-statements/administrative-sql-statements/show/extended-show/).

If the data types differ from what you expect them to be based on a
<code class="highlight fixed" style="white-space:pre-wrap">CREATE TABLE</code> statement, note that MariaDB sometimes changes
data types when you create or alter a table. The conditions under which this
occurs are described in the [Silent Column Changes](/sql-statements-structure/sql-statements/data-definition/create/silent-column-changes/) article.

The <code class="highlight fixed" style="white-space:pre-wrap">FULL</code> keyword causes the output to include the column
collation and comments, as well as the privileges you have for each column.

You can use <code class="highlight fixed" style="white-space:pre-wrap">db_name.tbl_name</code> as an alternative to the
<code class="highlight fixed" style="white-space:pre-wrap">tbl_name FROM db_name</code> syntax. In other words, these two
statements are equivalent:

```sql
SHOW COLUMNS FROM mytable FROM mydb;
SHOW COLUMNS FROM mydb.mytable;
```

<code class="highlight fixed" style="white-space:pre-wrap">SHOW COLUMNS</code> displays the following values for each table
column:

<strong>Field</strong> indicates the column name.

<strong>Type</strong> indicates the column data type.

<strong>Collation</strong> indicates the collation for non-binary string columns, or
NULL for other columns. This value is displayed only if you use the
FULL keyword.

The <strong>Null</strong> field contains YES if NULL values can be stored in the column,
NO if not.

The <strong>Key</strong> field indicates whether the column is indexed:

- If <strong>Key</strong> is empty, the column either is not indexed or is indexed only as a
  secondary column in a multiple-column, non-unique index.
- If <strong>Key</strong> is <em><strong>PRI</strong></em>, the column is a <code class="highlight fixed" style="white-space:pre-wrap">PRIMARY KEY</code> or
  is one of the columns in a multiple-column <code class="highlight fixed" style="white-space:pre-wrap">PRIMARY KEY</code>.
- If <strong>Key</strong> is <em><strong>UNI</strong></em>, the column is the first column of a unique-valued
  index that cannot contain <code class="highlight fixed" style="white-space:pre-wrap">NULL</code> values.
- If <strong>Key</strong> is <em><strong>MUL</strong></em>, multiple occurrences of a given value are allowed
  within the column. The column is the first column of a non-unique index or a
  unique-valued index that can contain <code class="highlight fixed" style="white-space:pre-wrap">NULL</code> values.

If more than one of the <strong>Key</strong> values applies to a given column of a
table, <strong>Key</strong> displays the one with the highest priority, in the order
PRI, UNI, MUL.

A <code class="highlight fixed" style="white-space:pre-wrap">UNIQUE</code> index may be displayed as <code class="highlight fixed" style="white-space:pre-wrap">PRI</code> if
it cannot contain <code class="highlight fixed" style="white-space:pre-wrap">NULL</code> values and there is no
<code class="highlight fixed" style="white-space:pre-wrap">PRIMARY KEY</code> in the table. A <code class="highlight fixed" style="white-space:pre-wrap">UNIQUE</code> index
may display as <code class="highlight fixed" style="white-space:pre-wrap">MUL</code> if several columns form a composite
<code class="highlight fixed" style="white-space:pre-wrap">UNIQUE</code> index; although the combination of the columns is
unique, each column can still hold multiple occurrences of a given value.

The <strong>Default</strong> field indicates the default value that is assigned to the
column.

The <strong>Extra</strong> field contains any additional information that is available about a given column.

<table><tbody><tr><th>Value</th><th>Description</th></tr>
<tr><td><code>AUTO_INCREMENT</code></td><td>The column was created with the <code>AUTO_INCREMENT</code> keyword.</td></tr>
<tr><td><code>PERSISTENT</code></td><td>The column was created with the <code>PERSISTENT</code> keyword. (New in 5.3)</td></tr>
<tr><td><code>VIRTUAL</code></td><td>The column was created with the <code>VIRTUAL</code> keyword. (New in 5.3)</td></tr>
<tr><td>on update <code>CURRENT_TIMESTAMP</code></td><td>The column is a <code>TIMESTAMP</code> column that is automatically updated on <code>INSERT</code> and <code>UPDATE</code>.</td></tr>
</tbody></table>

<strong>Privileges</strong> indicates the privileges you have for the column. This
value is displayed only if you use the <code class="highlight fixed" style="white-space:pre-wrap">FULL</code> keyword.

<strong>Comment</strong> indicates any comment the column has. This value is displayed
only if you use the <code class="highlight fixed" style="white-space:pre-wrap">FULL</code> keyword.

<code class="highlight fixed" style="white-space:pre-wrap">SHOW FIELDS</code> is a synonym for
<code class="highlight fixed" style="white-space:pre-wrap">SHOW COLUMNS</code>. Also [DESCRIBE](/sql-statements-structure/sql-statements/administrative-sql-statements/describe/) and [EXPLAIN](/sql-statements-structure/sql-statements/administrative-sql-statements/analyze-and-explain-statements/explain/) can be used as shortcuts.

You can also list a table's columns with:

```sql
mysqlshow db_name tbl_name
```

See the [mysqlshow](/clients-utilities/mysqlshow/) command for more details.

The [DESCRIBE](/sql-statements-structure/sql-statements/administrative-sql-statements/describe/) statement provides information similar to `SHOW COLUMNS`. The <a undefined>information_schema.COLUMNS</a> table provides similar, but more complete, information.

The [SHOW CREATE TABLE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-table/), [SHOW TABLE STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-table-status/), and [SHOW INDEX](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-index/) statements also provide information about tables.

## Examples

```sql
SHOW COLUMNS FROM city;
+------------+----------+------+-----+---------+----------------+
| Field      | Type     | Null | Key | Default | Extra          |
+------------+----------+------+-----+---------+----------------+
| Id         | int(11)  | NO   | PRI | NULL    | auto_increment |
| Name       | char(35) | NO   |     |         |                |
| Country    | char(3)  | NO   | UNI |         |                |
| District   | char(20) | YES  | MUL |         |                |
| Population | int(11)  | NO   |     | 0       |                |
+------------+----------+------+-----+---------+----------------+
```

```sql
SHOW COLUMNS FROM employees WHERE Type LIKE 'Varchar%';
+---------------+-------------+------+-----+---------+-------+
| Field         | Type        | Null | Key | Default | Extra |
+---------------+-------------+------+-----+---------+-------+
| first_name    | varchar(30) | NO   | MUL | NULL    |       |
| last_name     | varchar(40) | NO   |     | NULL    |       |
| position      | varchar(25) | NO   |     | NULL    |       |
| home_address  | varchar(50) | NO   |     | NULL    |       |
| home_phone    | varchar(12) | NO   |     | NULL    |       |
| employee_code | varchar(25) | NO   | UNI | NULL    |       |
+---------------+-------------+------+-----+---------+-------+
```

## See Also

- [DESCRIBE](/sql-statements-structure/sql-statements/administrative-sql-statements/describe/)
- [mysqlshow](/clients-utilities/mysqlshow/)
- [SHOW CREATE TABLE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-table/)
- [SHOW TABLE STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-table-status/)
- [SHOW INDEX](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-index/)
- [Extended SHOW](/sql-statements-structure/sql-statements/administrative-sql-statements/show/extended-show/)
- [Silent Column Changes](/sql-statements-structure/sql-statements/data-definition/create/silent-column-changes/)