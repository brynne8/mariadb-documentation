# INT2

`INT2` is a synonym for [SMALLINT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/smallint).

```sql
CREATE TABLE t1 (x INT2);

DESC t1;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| x     | smallint(6) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
```