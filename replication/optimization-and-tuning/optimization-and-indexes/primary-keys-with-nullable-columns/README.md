# Primary Keys with Nullable Columns

##### MariaDB starting with [10.1.7](/kb/en/mariadb-1017-release-notes/)

[MariaDB 10.1.7](/kb/en/mariadb-1017-release-notes/) introduced new behavior for dealing with primary keys over nullable columns.

Take the following table structure:

```sql
CREATE TABLE t1(
  c1 INT NOT NULL AUTO_INCREMENT, 
  c2 INT NULL DEFAULT NULL, 
  PRIMARY KEY(c1,c2)
);
```

Column c2 is part of a primary key, and thus it cannot be [NULL](/columns-storage-engines-and-plugins/data-types/null-values).

Before [MariaDB 10.1.7](/kb/en/mariadb-1017-release-notes/), MariaDB (as well as versions of MySQL before MySQL 5.7) would silently convert it into a NOT NULL column with a default value of <em>0</em>.

Since [MariaDB 10.1.7](/kb/en/mariadb-1017-release-notes/), the column is converted to NOT NULL, but without a default value. If we then attempt to insert a record without explicitly setting <em>c2</em>, a warning (or, in  strict mode, an error), will be thrown, for example:

```sql
INSERT INTO t1() VALUES();
Query OK, 1 row affected, 1 warning (0.00 sec)
Warning (Code 1364): Field 'c2' doesn't have a default value

SELECT * FROM t1;
+----+----+
| c1 | c2 |
+----+----+
|  1 |  0 |
+----+----+
```

MySQL, since 5.7, will abort such a CREATE TABLE with an error.

The [MariaDB 10.1.7](/kb/en/mariadb-1017-release-notes/) behavior adheres to the SQL 2003 standard.

SQL-2003, Part II, “Foundation” says:

<strong>11.7 &lt;unique constraint definition&gt;</strong><br><strong>Syntax Rules</strong>

…

<em>5) If the &lt;unique specification&gt; specifies PRIMARY KEY, then for each &lt;column name&gt; in the explicit or implicit &lt;unique column list&gt; for which NOT NULL is not specified, NOT NULL is implicit in the &lt;column definition&gt;.</em>

Essentially this means that all PRIMARY KEY columns are automatically converted to NOT NULL. Furthermore:

<strong>11.5 &lt;default clause&gt;</strong><br>
<strong>General Rules</strong>

…

<em>3) When a site S is set to its default value,</em>

…

<em>b) If the data descriptor for the site includes a &lt;default option&gt;, then S is set to the value specified by that &lt;default option&gt;.</em>

…

<em>e) Otherwise, S is set to the null value.</em>

There is no concept of “no default value” in the standard. Instead, a column always has an implicit default value of NULL. On insertion it might however fail the NOT NULL constraint. MariaDB and MySQL instead mark such a column as “not having a default value”. The end result is the same — a value must be specified explicitly or an INSERT will fail.

MariaDB since 10.1.7 behaves in a standard compatible manner — being part of a PRIMARY KEY, the nullable column gets an automatic NOT NULL constraint, on insertion one must specify a value for such a column. MariaDB before 10.1.7 was automatically assigning a default value of 0 — this behavior was non-standard. Issuing an error at CREATE TABLE time is also non-standard.

## See Also

- [MDEV-12248](https://jira.mariadb.org/browse/MDEV-12248) describes an edge-case that may result in replication problems when replicating from a master server before this change to a slave server after this change.