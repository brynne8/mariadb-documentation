# SPATIAL INDEX

## Description

On [MyISAM](/kb/en/myisam/) and [Aria](/columns-storage-engines-and-plugins/storage-engines/aria) tables, as well as on InnoDB tables from [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/), MariaDB can create spatial indexes (an R-tree index) using syntax similar to that for creating regular indexes, but extended with the `SPATIAL` keyword.
Currently, columns in spatial indexes must be declared `NOT NULL`.

Spatial indexes can be created when the table is created, or added after the fact like so:

- with [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table): <pre class="fixed"><span class="k">CREATE</span> <span class="k">TABLE</span> <span class="nf">geom</span> <span class="p">(</span><span class="n">g</span> <span class="n">GEOMETRY</span> <span class="k">NOT</span> <span class="no">NULL</span><span class="p">,</span> <span class="k">SPATIAL</span> <span class="k">INDEX</span><span class="p">(</span><span class="n">g</span><span class="p">));</span>
</pre>
- with [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table): <pre class="fixed"><span class="k">ALTER</span> <span class="k">TABLE</span> <span class="n">geom</span> <span class="k">ADD</span> <span class="k">SPATIAL</span> <span class="k">INDEX</span><span class="p">(</span><span class="n">g</span><span class="p">);</span>
</pre>
- with [CREATE INDEX](/sql-statements-structure/sql-statements/data-definition/create/create-index): <pre class="fixed"><span class="k">CREATE</span> <span class="k">SPATIAL</span> <span class="k">INDEX</span> <span class="n">sp_index</span> <span class="k">ON</span> <span class="nf">geom</span> <span class="p">(</span><span class="n">g</span><span class="p">);</span>
</pre>

`SPATIAL INDEX` creates an `R-tree` index. For storage
engines that support non-spatial indexing of spatial columns, the engine
creates a `B-tree` index. A `B-tree` index on spatial values is useful for
exact-value lookups, but not for range scans.

For more information on indexing spatial columns, see [CREATE INDEX](/sql-statements-structure/sql-statements/data-definition/create/create-index).

To drop spatial indexes, use [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table) or [DROP INDEX](/sql-statements-structure/sql-statements/data-definition/drop/drop-index):

- with [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table): <pre class="fixed"><span class="k">ALTER</span> <span class="k">TABLE</span> <span class="n">geom</span> <span class="k">DROP</span> <span class="k">INDEX</span> <span class="n">g</span><span class="p">;</span>
</pre>
- with [DROP INDEX](/sql-statements-structure/sql-statements/data-definition/drop/drop-index): <pre class="fixed"><span class="k">DROP</span> <span class="k">INDEX</span> <span class="n">sp_index</span> <span class="k">ON</span> <span class="n">geom</span><span class="p">;</span>
</pre>

### Data-at-Rest Encyption

Before [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/), InnoDB's spatial indexes could not be [encrypted](/kb/en/encryption/). If an InnoDB table was encrypted and if it contained spatial indexes, then those indexes would be unencrypted.

In [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/) and later, if <a undefined>innodb_checksum_algorithm</a> is set to `full_crc32` or `strict_full_crc32`, and if the table does not use <a undefined>ROW_FORMAT=COMPRESSED</a>, then InnoDB spatial indexes will be encrypted if the table is encrypted.

See [MDEV-12026](https://jira.mariadb.org/browse/MDEV-12026) for more information.