# INT1

`INT1` is a synonym for [TINYINT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/tinyint/).

```sql
CREATE TABLE t1 (x INT1);

DESC t1;
+-------+------------+------+-----+---------+-------+
| Field | Type       | Null | Key | Default | Extra |
+-------+------------+------+-----+---------+-------+
| x     | tinyint(4) | YES  |     | NULL    |       |
+-------+------------+------+-----+---------+-------+
```