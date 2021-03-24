# Trigger Overview

A trigger, as its name suggests, is a set of statements that run, or are triggered, when an event occurs on a table.

## Events

The event can be an INSERT, an UPDATE or a DELETE. The trigger can be executed BEFORE or AFTER the event. Until [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/), a table could have only one trigger defined for each event/timing combination: for example, a table could only have one BEFORE INSERT trigger.

The [LOAD DATA INFILE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/load-data-into-tables-or-index/load-data-infile/) and [LOAD XML](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/load-data-into-tables-or-index/load-xml/) statements invoke INSERT triggers for each row that is being inserted.

The [REPLACE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/replace/) statement is executed with the following workflow:

- BEFORE INSERT;
- BEFORE DELETE (only if a row is being deleted);
- AFTER DELETE (only if a row is being deleted);
- AFTER INSERT.

The [INSERT ... ON DUPLICATE KEY UPDATE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-on-duplicate-key-update/) statement, when a row already exists, follows the following workflow:

- BEFORE INSERT;
- BEFORE UPDATE;
- AFTER UPDATE.

Otherwise, it works like a normal INSERT statement.

Note that [TRUNCATE TABLE](/sql-statements-structure/sql-statements/table-statements/truncate-table/) does not activate any triggers.

## Triggers and errors

With non-transactional storage engines, if a BEFORE statement produces an error, the statement will not be executed. Statements that affect multiple rows will fail before inserting the current row.

With transactional engines, triggers are executed in the same transaction as the statement that invoked them.

If a warning is issued with the SIGNAL or RESIGNAL statement (that is, an error with an SQLSTATE starting with '01'), it will be treated like an error.

## Creating a trigger

Here's a simple example to demonstrate a trigger in action. Using these two tables as an example:

```sql
CREATE TABLE animals (id mediumint(9) 
NOT NULL AUTO_INCREMENT, 
name char(30) NOT NULL, 
PRIMARY KEY (`id`));

CREATE TABLE animal_count (animals int);

INSERT INTO animal_count (animals) VALUES(0);
```

We want to increment a counter each time a new animal is added. Here's what the trigger will look like:

```sql
CREATE TRIGGER increment_animal 
AFTER INSERT ON animals 
FOR EACH ROW 
UPDATE animal_count SET animal_count.animals = animal_count.animals+1;
```

The trigger has:

- a <em>name</em> (in this case `increment_animal`)
- a trigger time (in this case <em>after</em> the specified trigger event)
- a trigger event (an `INSERT`)
- a table with which it is associated (`animals`)
- a set of statements to run (here, just the one UPDATE statement)

`AFTER INSERT` specifies that the trigger will run <em>after</em> an `INSERT`. The trigger could also be set to run <em>before</em>, and the statement causing the trigger could be a `DELETE` or an `UPDATE` as well.

Now, if we insert a record into the `animals` table, the trigger will run, incrementing the animal_count table;

```sql
SELECT * FROM animal_count;
+---------+
| animals |
+---------+
|       0 |
+---------+

INSERT INTO animals (name) VALUES('aardvark');
INSERT INTO animals (name) VALUES('baboon');

SELECT * FROM animal_count;
+---------+
| animals |
+---------+
|       2 |
+---------+
```

For more details on the syntax, see [CREATE TRIGGER](/programming-customizing-mariadb/triggers-events/triggers/create-trigger/).

## Dropping triggers

To drop a trigger, use the [DROP TRIGGER](/sql-statements-structure/sql-statements/data-definition/drop/drop-trigger/) statement. Triggers are also dropped if the table with which they are associated is also dropped.

```sql
 DROP TRIGGER increment_animal; 
```

## Triggers metadata

The [Information Schema TRIGGERS Table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-triggers-table/) stores information about triggers.

The [SHOW TRIGGERS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-triggers/) statement returns similar information.

The [SHOW CREATE TRIGGER](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-trigger/) statement returns a CREATE TRIGGER statement that creates the given trigger.

## More complex triggers

Triggers can consist of multiple statements enclosed by a [BEGIN and END](/programming-customizing-mariadb/programmatic-compound-statements/begin-end/). If you're entering multiple statements on the command line, you'll want to temporarily set a new delimiter so that you can use a semicolon to delimit the statements inside your trigger. See [Delimiters in the mysql client](/kb/en/delimiters-in-the-mysql-client/) for more.

```sql
DROP TABLE animals;

UPDATE animal_count SET animals=0;

CREATE TABLE animals (id mediumint(9) NOT NULL AUTO_INCREMENT, 
name char(30) NOT NULL, 
PRIMARY KEY (`id`)) 
ENGINE=InnoDB;

DELIMITER //
CREATE TRIGGER the_mooses_are_loose
AFTER INSERT ON animals
FOR EACH ROW
BEGIN
 IF NEW.name = 'Moose' THEN
  UPDATE animal_count SET animal_count.animals = animal_count.animals+100;
 ELSE 
  UPDATE animal_count SET animal_count.animals = animal_count.animals+1;
 END IF;
END; //

DELIMITER ;

INSERT INTO animals (name) VALUES('Aardvark');

SELECT * FROM animal_count;
+---------+
| animals |
+---------+
|       1 |
+---------+

INSERT INTO animals (name) VALUES('Moose');

SELECT * FROM animal_count;
+---------+
| animals |
+---------+
|     101 |
+---------+
```

## Trigger errors

If a trigger contains an error and the engine is transactional, or it is a BEFORE trigger, the trigger will not run, and will prevent the original statement from running as well. If the engine is non-transactional, and it is an AFTER trigger, the trigger will not run, but the original statement will.

Here, we'll drop the above examples, and then recreate the trigger with an error, a field that doesn't exist, first using  the default [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/), a transactional engine, and then again using [MyISAM](/kb/en/myisam/), a non-transactional engine.

```sql
DROP TABLE animals;

CREATE TABLE animals (id mediumint(9) NOT NULL AUTO_INCREMENT, 
name char(30) NOT NULL, 
PRIMARY KEY (`id`)) 
ENGINE=InnoDB;

CREATE TRIGGER increment_animal 
AFTER INSERT ON animals 
FOR EACH ROW 
UPDATE animal_count SET animal_count.id = animal_count_id+1;

INSERT INTO animals (name) VALUES('aardvark');
ERROR 1054 (42S22): Unknown column 'animal_count.id' in 'field list'

SELECT * FROM animals;
Empty set (0.00 sec)
```

And now the identical procedure, but with a MyISAM table.

```sql
DROP TABLE animals;

CREATE TABLE animals (id mediumint(9) NOT NULL AUTO_INCREMENT, 
name char(30) NOT NULL, 
PRIMARY KEY (`id`)) 
ENGINE=MyISAM;

CREATE TRIGGER increment_animal 
AFTER INSERT ON animals 
FOR EACH ROW 
UPDATE animal_count SET animal_count.id = animal_count_id+1;

INSERT INTO animals (name) VALUES('aardvark');
ERROR 1054 (42S22): Unknown column 'animal_count.id' in 'field list'

SELECT * FROM animals;
+----+----------+
| id | name     |
+----+----------+
|  1 | aardvark |
+----+----------+
```

The following example shows how to use a trigger to validate data. The [SIGNAL](/programming-customizing-mariadb/programmatic-compound-statements/signal/) statement is used to intentionally produce an error if the email field is not a valid email. As the example shows, in that case the new row is not inserted (because it is a BEFORE trigger).

```sql
CREATE TABLE user (
	id INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
	first_name CHAR(20),
	last_name CHAR(20),
	email CHAR(100)
)
	ENGINE = MyISAM;

DELIMITER //
CREATE TRIGGER bi_user
  BEFORE INSERT ON user
  FOR EACH ROW
BEGIN
  IF NEW.email NOT LIKE '_%@_%.__%' THEN
    SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Email field is not valid';
  END IF;
END; //
DELIMITER ;

INSERT INTO user (first_name, last_name, email) VALUES ('John', 'Doe', 'john_doe.example.net');
ERROR 1644 (45000): Email field is not valid

SELECT * FROM user;
Empty set (0.00 sec)
```

## See also

- [CREATE TRIGGER](/programming-customizing-mariadb/triggers-events/triggers/create-trigger/)
- [DROP TRIGGER](/sql-statements-structure/sql-statements/data-definition/drop/drop-trigger/)
- [Information Schema TRIGGERS Table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-triggers-table/)
- [SHOW TRIGGERS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-triggers/)
- [SHOW CREATE TRIGGER](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-trigger/)
- [Trigger Limitations](/programming-customizing-mariadb/triggers-events/triggers/trigger-limitations/)