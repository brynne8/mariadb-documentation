# OQGRAPH Examples

## Creating a Table with origid, destid Only

```sql
CREATE TABLE oq_backing (
  origid INT UNSIGNED NOT NULL, 
  destid INT UNSIGNED NOT NULL,  
  PRIMARY KEY (origid, destid), 
  KEY (destid)
);
```

Some data can be inserted into the backing table to test with later:

```sql
INSERT INTO oq_backing(origid, destid) 
 VALUES (1,2), (2,3), (3,4), (4,5), (2,6), (5,6);
```

Now the read-only [OQGRAPH](/kb/en/oqgraph/) table is created.

From [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/) onwards you can use the following syntax:

```sql
CREATE TABLE oq_graph
ENGINE=OQGRAPH 
data_table='oq_backing' origid='origid' destid='destid';
```

Prior to [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/), the [CREATE](/sql-statements-structure/sql-statements/data-definition/create/create-table/) statement must match the format below - any difference will result in an error.

```sql
CREATE TABLE oq_graph (
  latch VARCHAR(32) NULL,
  origid BIGINT UNSIGNED NULL,
  destid BIGINT UNSIGNED NULL,
  weight DOUBLE NULL,
  seq BIGINT UNSIGNED NULL,
  linkid BIGINT UNSIGNED NULL,
  KEY (latch, origid, destid) USING HASH,
  KEY (latch, destid, origid) USING HASH
) 
ENGINE=OQGRAPH 
data_table='oq_backing' origid='origid' destid='destid';
```

## Creating a Table with Weight

For the examples on this page, we'll create a second OQGRAPH table and backing table, this time with `weight` as well.

```sql
CREATE TABLE oq2_backing (
  origid INT UNSIGNED NOT NULL, 
  destid INT UNSIGNED NOT NULL, 
  weight DOUBLE NOT NULL, 
  PRIMARY KEY (origid, destid), 
  KEY (destid)
);
```

```sql
INSERT INTO oq2_backing(origid, destid, weight)  
 VALUES (1,2,1), (2,3,1), (3,4,3), (4,5,1), (2,6,10), (5,6,2);
```

```sql
CREATE TABLE oq2_graph (
  latch VARCHAR(32) NULL,
  origid BIGINT UNSIGNED NULL,
  destid BIGINT UNSIGNED NULL,
  weight DOUBLE NULL,
  seq BIGINT UNSIGNED NULL,
  linkid BIGINT UNSIGNED NULL,
  KEY (latch, origid, destid) USING HASH,
  KEY (latch, destid, origid) USING HASH
) 
ENGINE=OQGRAPH 
data_table='oq2_backing' origid='origid' destid='destid' weight='weight';
```

## Shortest Path

A `latch` value of `'dijkstras'` and an `origid` and `destid` is used for finding the shortest path between two nodes, for example:

```sql
SELECT * FROM oq_graph WHERE latch='breadth_first' AND origid=1 AND destid=6;
+----------+--------+--------+--------+------+--------+
| latch    | origid | destid | weight | seq  | linkid |
+----------+--------+--------+--------+------+--------+
| dijkstras|      1 |      6 |   NULL |    0 |      1 |
| dijkstras|      1 |      6 |      1 |    1 |      2 |
| dijkstras|      1 |      6 |      1 |    2 |      6 |
+----------+--------+--------+--------+------+--------+
```

Note that nodes are uni-directional, so there is no path from node 6 to node 1:

```sql
SELECT * FROM oq_graph WHERE latch='dijkstras' AND origid=6 AND destid=1;
Empty set (0.00 sec)
```

Using the [GROUP_CONCAT](/built-in-functions/aggregate-functions/group_concat/) function can produce more readable results, for example:

```sql
SELECT GROUP_CONCAT(linkid ORDER BY seq) AS path FROM oq_graph 
 WHERE latch='dijkstras' AND origid=1 AND destid=6;
+-------+
| path  |
+-------+
| 1,2,6 |
+-------+
```

Using the table `oq2_graph`, the shortest path is different:

```sql
SELECT GROUP_CONCAT(linkid ORDER BY seq) AS path FROM oq2_graph 
 WHERE latch='dijkstras' AND origid=1 AND destid=6;
+-------------+
| path        |
+-------------+
| 1,2,3,4,5,6 |
+-------------+
```

The reason is the weight between nodes 2 and 6 is `10` in `oq_graph2`, so the shortest path taking into account `weight` is now across more nodes.

## Possible Destinations

```sql
SELECT GROUP_CONCAT(linkid) AS dests FROM oq_graph WHERE latch='dijkstras' AND origid=2;
+-----------+
| dests     |
+-----------+
| 5,4,6,3,2 |
+-----------+
```

Note that this returns all possible destinations along the path, not just immediate links.

## Leaf Nodes

##### MariaDB [10.3.3](/kb/en/mariadb-1033-release-notes/)

Support for the `leaves` latch value was introduced in [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/).

A `latch` value of `'leaves'` and either `origid` or `destid` is used for finding leaf nodes at the beginning or end of a graph.

```sql
INSERT INTO oq_backing(origid, destid)  
 VALUES (1,2), (2,3), (3,5), (4,5), (5,6), (6,7), (6,8), (2,8);
```

For example, to find all reachable nodes from `origid` that only have incoming edges:

```sql
SELECT * FROM oq_graph WHERE latch='leaves' AND origid=2;
+--------+--------+--------+--------+------+--------+
| latch  | origid | destid | weight | seq  | linkid |
+--------+--------+--------+--------+------+--------+
| leaves |      2 |   NULL |      4 |    2 |      7 |
| leaves |      2 |   NULL |      1 |    1 |      8 |
+--------+--------+--------+--------+------+--------+
```

And to find all nodes from which a path can be found to `destid` that only have outgoing edges:

```sql
SELECT * FROM oq_graph WHERE latch='leaves' AND destid=5;
+--------+--------+--------+--------+------+--------+
| latch  | origid | destid | weight | seq  | linkid |
+--------+--------+--------+--------+------+--------+
| leaves |   NULL |      5 |      3 |    2 |      1 |
| leaves |   NULL |      5 |      1 |    1 |      4 |
+--------+--------+--------+--------+------+--------+
```

## Summary of Implemented Latch Commands

<table><tbody><tr><th>Latch</th><th>Alternative</th><th>additional where clause fields</th><th>Graph operation</th></tr>
<tr><td>NULL</td><td>(unspecified)</td><td>(none)</td><td>List original data</td></tr>
<tr><td>(empty string)</td><td>0</td><td>(none extra)</td><td>List all vertices in linkid column</td></tr>
<tr><td>(empty string)</td><td>0</td><td>origid</td><td>List all first hop vertices from origid in linkid column</td></tr>
<tr><td>dijkstras</td><td>1</td><td>origid, destid</td><td>Find shortest path using Dijkstras algorithm between origid and destid, with traversed vertex ids in linkid column</td></tr>
<tr><td>dijkstras</td><td>1</td><td>origid</td><td>Find all vertices reachable from origid, listed in linkid column, and report sum of weights of vertices on path to given vertex in weight</td></tr>
<tr><td>dijkstras</td><td>1</td><td>destid</td><td>Find all vertices from which a path can be found to destid, listed in linkid column, and report sum of weights of vertices on path to given vertex in weight</td></tr>
<tr><td>breadth_first</td><td>2</td><td>origid</td><td>List vertices reachable from origid in linkid column</td></tr>
<tr><td>breadth_first</td><td>2</td><td>destid</td><td>List vertices from which a path can be found to destid in linkid column</td></tr>
<tr><td>breadth_first</td><td>2</td><td>origid, destid</td><td>Find shortest path between origid and destid, report in linkid column</td></tr>
<tr><td>leaves</td><td>4</td><td>origid</td><td>List vertices reachable from origid, that only have incoming edges (from <a href="/kb/en/mariadb-1033-release-notes/">MariaDB 10.3.3</a>)</td></tr>
<tr><td>leaves</td><td>4</td><td>destid</td><td>List vertices from which a path can be found to destid, that only have outgoing edges (from <a href="/kb/en/mariadb-1033-release-notes/">MariaDB 10.3.3</a>)</td></tr>
<tr><td>leaves</td><td>4</td><td>origid, destid</td><td>Not supported, will return an empty result</td></tr>
</tbody></table>

---

Note: the use of integer latch commands is deprecated and may be phased out in a future release. Currently, numeric values in the strings are interpreted as aliases, and use of an integer column can be optionally allowed, for the latch commands column.

The use of integer latches is controlled using the [oqgraph_allow_create_integer_latch](/kb/en/oqgraph-system-and-status-variables/#oqgraph_allow_create_integer_latch) system variable.

## See Also

- [OQGRAPH Overview](/columns-storage-engines-and-plugins/storage-engines/oqgraph-storage-engine/oqgraph-overview/)