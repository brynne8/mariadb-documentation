# SHOW INDEX

## Syntax

```sql
SHOW {INDEX | INDEXES | KEYS} 
 FROM tbl_name [FROM db_name]
 [WHERE expr]
```

## Description

<code class="highlight fixed" style="white-space:pre-wrap">SHOW INDEX</code> returns table index information. The format
resembles that of the SQLStatistics call in ODBC.

You can use <code class="highlight fixed" style="white-space:pre-wrap">db_name.tbl_name</code> as an alternative to the
 <code class="highlight fixed" style="white-space:pre-wrap">tbl_name FROM db_name</code> syntax. These two statements are
 equivalent:

```sql
SHOW INDEX FROM mytable FROM mydb;
SHOW INDEX FROM mydb.mytable;
```

<code class="highlight fixed" style="white-space:pre-wrap">SHOW KEYS</code> and <code class="highlight fixed" style="white-space:pre-wrap">SHOW INDEXES</code> are synonyms for <code class="highlight fixed" style="white-space:pre-wrap">SHOW INDEX</code>.

You can also list a table's indexes with the following command:

```sql
mysqlshow -k db_name tbl_name
```

See [mysqlshow](/clients-utilities/mysqlshow) for more details.

The <a undefined>information_schema.STATISTICS</a> table stores similar information.

The following fields are returned by `SHOW INDEX`.

<table><tbody><tr><th>Field</th><th>Description</th></tr>
<tr><td><strong><code>Table</code></strong></td><td>Table name</td></tr>
<tr><td><strong><code>Non_unique</code></strong></td><td><code>1</code> if the index permits duplicate values, <code>0</code> if values must be unique.</td></tr>
<tr><td><strong><code>Key_name</code></strong></td><td>Index name. The primary key is always named <code>PRIMARY</code>.</td></tr>
<tr><td><strong><code>Seq_in_index</code></strong></td><td>The column's sequence in the index, beginning with <code>1</code>.</td></tr>
<tr><td><strong><code>Column_name</code></strong></td><td>Column name.</td></tr>
<tr><td><strong><code>Collation</code></strong></td><td>Either <code>A</code>, if the column is sorted in ascending order in the index, or <code>NULL</code> if it's not sorted.</td></tr>
<tr><td><strong><code>Cardinality</code></strong></td><td>Estimated number of unique values in the index. The cardinality statistics are calculated at various times, and can help the optimizer make improved decisions.</td></tr>
<tr><td><strong><code>Sub_part</code></strong></td><td><code>NULL</code> if the entire column is included in the index, or the number of included characters if not.</td></tr>
<tr><td><strong><code>Packed</code></strong></td><td><code>NULL</code> if the index is not packed, otherwise how the index is packed.</td></tr>
<tr><td><strong><code>Null</code></strong></td><td><code>NULL</code> if <code>NULL</code> values are permitted in the column, an empty string if <code>NULL</code>'s are not permitted.</td></tr>
<tr><td><strong><code>Index_type</code></strong></td><td>The index type, which can be <code>BTREE</code>, <code>FULLTEXT</code>, <code>HASH</code> or <code>RTREE</code>. See <a href="/kb/en/storage-engine-index-types/">Storage Engine Index Types</a>.</td></tr>
<tr><td><strong><code>Comment</code></strong></td><td>Other information, such as whether the index is disabled.</td></tr>
<tr><td><strong><code>Index_comment</code></strong></td><td>Contents of the <code>COMMENT</code> attribute when the index was created.</td></tr>
</tbody></table>

The <code class="highlight fixed" style="white-space:pre-wrap">WHERE</code> and <code class="highlight fixed" style="white-space:pre-wrap">LIKE</code> clauses can be given to select rows using more general conditions, as discussed in [Extended SHOW](/sql-statements-structure/sql-statements/administrative-sql-statements/show/extended-show).

## Examples

```sql
CREATE TABLE IF NOT EXISTS `employees_example` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `first_name` varchar(30) NOT NULL,
  `last_name` varchar(40) NOT NULL,
  `position` varchar(25) NOT NULL,
  `home_address` varchar(50) NOT NULL,
  `home_phone` varchar(12) NOT NULL,
  `employee_code` varchar(25) NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `employee_code` (`employee_code`),
  KEY `first_name` (`first_name`,`last_name`)
) ENGINE=Aria;

INSERT INTO `employees_example` (`first_name`, `last_name`, `position`, `home_address`, `home_phone`, `employee_code`)
  VALUES
  ('Mustapha', 'Mond', 'Chief Executive Officer', '692 Promiscuous Plaza', '326-555-3492', 'MM1'),
  ('Henry', 'Foster', 'Store Manager', '314 Savage Circle', '326-555-3847', 'HF1'),
  ('Bernard', 'Marx', 'Cashier', '1240 Ambient Avenue', '326-555-8456', 'BM1'),
  ('Lenina', 'Crowne', 'Cashier', '281 Bumblepuppy Boulevard', '328-555-2349', 'LC1'),
  ('Fanny', 'Crowne', 'Restocker', '1023 Bokanovsky Lane', '326-555-6329', 'FC1'),
  ('Helmholtz', 'Watson', 'Janitor', '944 Soma Court', '329-555-2478', 'HW1');

SHOW INDEXES FROM employees_example;
+-------------------+------------+---------------+--------------+---------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
| Table             | Non_unique | Key_name      | Seq_in_index | Column_name   | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment |
+-------------------+------------+---------------+--------------+---------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
| employees_example |          0 | PRIMARY       |            1 | id            | A         |           7 |     NULL | NULL   |      | BTREE      |         |               |
| employees_example |          0 | employee_code |            1 | employee_code | A         |           7 |     NULL | NULL   |      | BTREE      |         |               |
| employees_example |          1 | first_name    |            1 | first_name    | A         |        NULL |     NULL | NULL   |      | BTREE      |         |               |
| employees_example |          1 | first_name    |            2 | last_name     | A         |        NULL |     NULL | NULL   |      | BTREE      |         |               |
+-------------------+------------+---------------+--------------+---------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
```