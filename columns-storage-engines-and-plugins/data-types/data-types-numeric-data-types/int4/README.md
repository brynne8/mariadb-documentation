# INT4

`INT4` is a synonym for [INT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/int/).

```sql
CREATE TABLE t1 (x INT4);

DESC t1;
+-------+---------+------+-----+---------+-------+
| Field | Type    | Null | Key | Default | Extra |
+-------+---------+------+-----+---------+-------+
| x     | int(11) | YES  |     | NULL    |       |
+-------+---------+------+-----+---------+-------+
```