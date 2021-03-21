# CONSTRAINT

MariaDB supports the implementation of constraints at the table-level using either [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/) or [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/) statements.  A table constraint restricts the data you can add to the table.  If you attempt to insert invalid data on a column, MariaDB throws an error.

## Syntax

```sql
[CONSTRAINT [symbol]] constraint_expression

constraint_expression:
  | PRIMARY KEY [index_type] (index_col_name, ...) [index_option] ...
  | FOREIGN KEY [index_name] (index_col_name, ...) 
       REFERENCES tbl_name (index_col_name, ...)
       [ON DELETE reference_option]
       [ON UPDATE reference_option]
  | UNIQUE [INDEX|KEY] [index_name]
       [index_type] (index_col_name, ...) [index_option] ...
  | CHECK (check_constraints)

index_type:
  USING {BTREE | HASH | RTREE}

index_col_name:
  col_name [(length)] [ASC | DESC]

index_option:
  | KEY_BLOCK_SIZE [=] value
  | index_type
  | WITH PARSER parser_name
  | COMMENT 'string'
  | CLUSTERING={YES|NO}

reference_option:
  RESTRICT | CASCADE | SET NULL | NO ACTION | SET DEFAULT
```

## Description

Constraints provide restrictions on the data you can add to a table.  This allows you to enforce data integrity from MariaDB, rather than through application logic.  When a statement violates a constraint, MariaDB throws an error.

There are four types of table constraints:

<table><tbody><tr><th>Constraint</th><th>Description</th></tr>
<tr><td><code>PRIMARY KEY</code></td><td>Sets the column for referencing rows.  Values must be unique and not null.</td></tr>
<tr><td><code>FOREIGN KEY</code></td><td>Sets the column to reference the primary key on another table.</td></tr>
<tr><td><code>UNIQUE</code></td><td>Requires values in column or columns only occur once in the table.</td></tr>
<tr><td><code>CHECK</code></td><td>Checks whether the data meets the given condition.</td></tr>
</tbody></table>

The [Information Schema TABLE_CONSTRAINTS Table](/kb/en/information-schema-table_constraints-table/) contains information about tables that have constraints.

### FOREIGN KEY Constraints

[InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) supports [foreign key](/replication/optimization-and-tuning/optimization-and-indexes/foreign-keys/) constraints. The syntax for a foreign key
constraint definition in InnoDB looks like this:

```sql
[CONSTRAINT [symbol]] FOREIGN KEY
    [index_name] (index_col_name, ...)
    REFERENCES tbl_name (index_col_name,...)
    [ON DELETE reference_option]
    [ON UPDATE reference_option]

reference_option:
    RESTRICT | CASCADE | SET NULL | NO ACTION
```

The [Information Schema REFERENTIAL_CONSTRAINTS](/kb/en/information-schema-referential_constraints-table/) table has more information about foreign keys.

### CHECK Constraints

##### MariaDB starting with [10.2.1](/kb/en/mariadb-1021-release-notes/)

From [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/), constraints are enforced. Before [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/) constraint expressions were accepted in the syntax but ignored.

In [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/) you can define constraints in 2 different ways:

- `CHECK(expression)` given as part of a column definition.
- `CONSTRAINT [constraint_name] CHECK (expression)`

Before a row is inserted or updated, all constraints are evaluated in the order they are defined. If any constraint expression returns false, then the row will not be inserted or updated.
One can use most deterministic functions in a constraint, including [UDFs](/programming-customizing-mariadb/user-defined-functions/).

```sql
CREATE TABLE t1 (a INT CHECK (a>2), b INT CHECK (b>2), CONSTRAINT a_greater CHECK (a>b));
```

If you use the second format and you don't give a name to the constraint, then the constraint will get an automatically generated name. This is done so that you can later delete the constraint with [ALTER TABLE DROP constraint_name](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/).

One can disable all constraint expression checks by setting the [check_constraint_checks](/kb/en/server-system-variables/#check_constraint_checks) variable to `OFF`. This is useful for example when loading a table that violates some constraints that you want to later find and fix in SQL.

### Replication

In [row-based](/kb/en/binary-log-formats/#row-based) [replication](/replication/), only the master checks constraints, and failed statements will not be replicated. In [statement-based](/kb/en/binary-log-formats/#statement-based) replication, the slaves will also check constraints. Constraints should therefore be identical, as well as deterministic, in a replication environment.

### Auto_increment

##### MariaDB starting with [10.2.6](/kb/en/mariadb-1026-release-notes/)

- From [MariaDB 10.2.6](/kb/en/mariadb-1026-release-notes/), [auto_increment](/columns-storage-engines-and-plugins/data-types/auto_increment/) columns are no longer permitted in check constraints. Previously they were permitted, but would not work correctly. See [MDEV-11117](https://jira.mariadb.org/browse/MDEV-11117).

## Examples

```sql
CREATE TABLE product (category INT NOT NULL, id INT NOT NULL,
                      price DECIMAL,
                      PRIMARY KEY(category, id)) ENGINE=INNODB;
CREATE TABLE customer (id INT NOT NULL,
                       PRIMARY KEY (id)) ENGINE=INNODB;
CREATE TABLE product_order (no INT NOT NULL AUTO_INCREMENT,
                            product_category INT NOT NULL,
                            product_id INT NOT NULL,
                            customer_id INT NOT NULL,
                            PRIMARY KEY(no),
                            INDEX (product_category, product_id),
                            FOREIGN KEY (product_category, product_id)
                              REFERENCES product(category, id)
                              ON UPDATE CASCADE ON DELETE RESTRICT,
                            INDEX (customer_id),
                            FOREIGN KEY (customer_id)
                              REFERENCES customer(id)) ENGINE=INNODB;
```

##### MariaDB starting with [10.2.1](/kb/en/mariadb-1021-release-notes/)

The following examples will work from [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/) onwards.

Numeric constraints and comparisons:

```sql
CREATE TABLE t1 (a INT CHECK (a>2), b INT CHECK (b>2), CONSTRAINT a_greater CHECK (a>b));

INSERT INTO t1(a) VALUES (1);
ERROR 4022 (23000): CONSTRAINT `a` failed for `test`.`t1`

INSERT INTO t1(a,b) VALUES (3,4);
ERROR 4022 (23000): CONSTRAINT `a_greater` failed for `test`.`t1`

INSERT INTO t1(a,b) VALUES (4,3);
Query OK, 1 row affected (0.04 sec)
```

Dropping a constraint:

```sql
ALTER TABLE t1 DROP CONSTRAINT a_greater;
```

Adding a constraint:

```sql
ALTER TABLE t1 ADD CONSTRAINT a_greater CHECK (a>b);
```

Date comparisons and character length:

```sql
CREATE TABLE t2 (name VARCHAR(30) CHECK (CHAR_LENGTH(name)>2), start_date DATE, 
  end_date DATE CHECK (start_date IS NULL OR end_date IS NULL OR start_date<end_date));

INSERT INTO t2(name, start_date, end_date) VALUES('Ione', '2003-12-15', '2014-11-09');
Query OK, 1 row affected (0.04 sec)

INSERT INTO t2(name, start_date, end_date) VALUES('Io', '2003-12-15', '2014-11-09');
ERROR 4022 (23000): CONSTRAINT `name` failed for `test`.`t2`

INSERT INTO t2(name, start_date, end_date) VALUES('Ione', NULL, '2014-11-09');
Query OK, 1 row affected (0.04 sec)

INSERT INTO t2(name, start_date, end_date) VALUES('Ione', '2015-12-15', '2014-11-09');
ERROR 4022 (23000): CONSTRAINT `end_date` failed for `test`.`t2`
```

A misplaced parenthesis:

```sql
CREATE TABLE t3 (name VARCHAR(30) CHECK (CHAR_LENGTH(name>2)), start_date DATE, 
  end_date DATE CHECK (start_date IS NULL OR end_date IS NULL OR start_date<end_date));
Query OK, 0 rows affected (0.32 sec)

INSERT INTO t3(name, start_date, end_date) VALUES('Io', '2003-12-15', '2014-11-09');
Query OK, 1 row affected, 1 warning (0.04 sec)

SHOW WARNINGS;
+---------+------+----------------------------------------+
| Level   | Code | Message                                |
+---------+------+----------------------------------------+
| Warning | 1292 | Truncated incorrect DOUBLE value: 'Io' |
+---------+------+----------------------------------------+
```

Compare the definition of table <em>t2</em> to table <em>t3</em>. `CHAR_LENGTH(name)&gt;2` is very different to `CHAR_LENGTH(name&gt;2)` as the latter mistakenly performs a numeric comparison on the <em>name</em> field, leading to unexpected results.

## See Also

- [Foreign Keys](/replication/optimization-and-tuning/optimization-and-indexes/foreign-keys/)