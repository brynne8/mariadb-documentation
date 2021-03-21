# INT8

`INT8` is a synonym for [BIGINT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/bigint).

```sql
CREATE TABLE t1 (x INT8);

DESC t1;
+-------+------------+------+-----+---------+-------+
| Field | Type       | Null | Key | Default | Extra |
+-------+------------+------+-----+---------+-------+
| x     | bigint(20) | YES  |     | NULL    |       |
+-------+------------+------+-----+---------+-------+
```