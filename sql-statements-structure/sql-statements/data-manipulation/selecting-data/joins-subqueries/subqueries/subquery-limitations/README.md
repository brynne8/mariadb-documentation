# Subquery Limitations

<br><br>
There are a number of limitations regarding [subqueries](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/joins-subqueries/subqueries/), which are discussed below.
<br><br>
The following tables and data will be used in the examples that follow: 
<br><br><br>

```sql
CREATE TABLE staff(name VARCHAR(10),age TINYINT);

CREATE TABLE customer(name VARCHAR(10),age TINYINT);
```

```sql
INSERT INTO staff VALUES 
('Bilhah',37), ('Valerius',61), ('Maia',25);

INSERT INTO customer VALUES 
('Thanasis',48), ('Valerius',61), ('Brion',51);
```

### LIMIT

MariaDB  doesn't support [LIMIT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/limit/) in [subqueries](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/joins-subqueries/subqueries/) with certain subquery operators. For example:

```sql
SELECT * FROM staff WHERE name IN (SELECT name FROM customer ORDER BY name);
+----------+------+
| name     | age  |
+----------+------+
| Valerius |   61 |
+----------+------+
```

is valid, but

```sql
SELECT * FROM staff WHERE name IN (SELECT NAME FROM customer ORDER BY name LIMIT 1);
ERROR 1235 (42000): This version of MariaDB doesn't 
  yet support 'LIMIT & IN/ALL/ANY/SOME subquery'
```

is not.

### Modifying and Selecting from the Same Table

It's not possible to both modify and select from the same table in a subquery. For example:

```sql
DELETE FROM staff WHERE name = (SELECT name FROM staff WHERE age=61);
ERROR 1093 (HY000): Table 'staff' is specified twice, both 
  as a target for 'DELETE' and as a separate source for data
```

### Row Comparison Operations

There is only partial support for row comparison operations. The expression in

```sql
expr op {ALL|ANY|SOME} subquery,
```

must be scalar and the subquery can only return a single column.

However, because of the way `IN` is implemented (it is rewritten as a sequence of `=` comparisons and `AND`), the expression in

```sql
expression [NOT] IN subquery
```

is permitted to be an n-tuple and the subquery can return rows of n-tuples.

For example:

```sql
SELECT * FROM staff WHERE (name,age) NOT IN (
  SELECT name,age FROM customer WHERE age >=51]
);
+--------+------+
| name   | age  |
+--------+------+
| Bilhah |   37 |
| Maia   |   25 |
+--------+------+
```

is permitted, but

```sql
SELECT * FROM staff WHERE (name,age) = ALL (
  SELECT name,age FROM customer WHERE age >=51
);
ERROR 1241 (21000): Operand should contain 1 column(s)
```

is not.

### Correlated Subqueries

Subqueries in the FROM clause cannot be correlated subqueries. They cannot be evaluated for each row of the outer query since they are evaluated to produce a result set during when the query is executed.

### Stored Functions

A subquery can refer to a [stored function](/programming-customizing-mariadb/stored-routines/stored-functions/) which modifies data. This is an extension to the SQL standard, but can result in indeterminate outcomes. For example, take:

```sql
SELECT ... WHERE x IN (SELECT f() ...);
```

where <em>f()</em> inserts rows. The function <em>f()</em> could be executed a different number of times depending on how the optimizer chooses to handle the query.

This sort of construct is therefore not safe to use in replication that is not [row-based](/mariadb-administration/server-monitoring-logs/binary-log/binary-log-formats/), as there could be different results on the master and the slave.