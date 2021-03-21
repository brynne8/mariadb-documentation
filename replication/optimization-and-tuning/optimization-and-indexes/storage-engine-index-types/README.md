# Storage Engine Index Types

This refers to the index_type definition when creating an index, i.e. BTREE, HASH or RTREE.

For more information on general types of indexes, such as primary keys, unique indexes etc, go to [Getting Started with Indexes](/replication/optimization-and-tuning/optimization-and-indexes/getting-started-with-indexes/).

<table><tbody><tr><th>Storage Engine</th><th>Permitted Indexes</th></tr>
<tr><td><a href="/kb/en/aria/">Aria</a></td><td>BTREE, RTREE</td></tr>
<tr><td><a href="/kb/en/myisam/">MyISAM</a></td><td>BTREE, RTREE</td></tr>
<tr><td><a href="/kb/en/innodb/">InnoDB</a></td><td>BTREE</td></tr>
<tr><td><a href="/kb/en/memory-storage-engine/">MEMORY/HEAP</a></td><td>HASH, BTREE</td></tr>
</tbody></table>

BTREE is generally the default index type. For [MEMORY](/replication/optimization-and-tuning/query-optimizations/guiduuid-performance/mariadb/memory-storage-engine/) tables, HASH is the default. [TokuDB](/columns-storage-engines-and-plugins/storage-engines/tokudb/) uses a particular data structure called <em>fractal trees</em>, which is optimized for data that do not entirely fit memory.

Understanding the B-tree and hash data structures can help predict how different queries perform on different storage engines that use these data structures in their indexes, particularly for the MEMORY storage engine that lets you choose B-tree or hash indexes.
B-Tree Index Characteristics

## B-tree Indexes

B-tree indexes are used for column comparisons using the &gt;, &gt;=, =, &gt;=, &lt; or BETWEEN operators, as well as for LIKE comparisons that begin with a constant.

For example, the query <code class="fixed" style="white-space:pre-wrap">SELECT * FROM Employees WHERE First_Name LIKE 'Maria%';</code> can make use of a B-tree index, while <code class="fixed" style="white-space:pre-wrap">SELECT * FROM Employees WHERE First_Name LIKE '%aria';</code> cannot.

B-tree indexes also permit leftmost prefixing for searching of rows.

If the number or rows doesn't change, hash indexes occupy a fixed amount of memory, which is lower than the memory occupied by BTREE indexes.

## Hash Indexes

Hash indexes, in contrast, can only be used for equality comparisons, so those using the = or &lt;=&gt; operators. They cannot be used for ordering, and provide no information to the optimizer on how many rows exist between two values.

Hash indexes do not permit leftmost prefixing - only the whole index can be used.

## R-tree Indexes

See [SPATIAL](/kb/en/spatial/) for more information.