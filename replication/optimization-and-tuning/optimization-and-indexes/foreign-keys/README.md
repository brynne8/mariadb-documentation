# Foreign Keys

## Overview

A foreign key is a constraint which can be used to enforce data integrity. It is composed by a column (or a set of columns) in a table called the child table, which references to a column (or a set of columns) in a table called the parent table. If foreign keys are used, MariaDB performs some checks to enforce that some integrity rules are always enforced. For a more exhaustive explanation, see [Relational databases: Foreign Keys](/kb/en/relational-databases-foreign-keys/).

Foreign keys can only be used with storage engines that support them. The default [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) and the obsolete [PBXT](/kb/en/pbxt/) support foreign keys.

[Partitioned tables](/kb/en/managing-mariadb-partitioning/) cannot contain foreign keys, and cannot be referenced by a foreign key.

## Syntax

<strong>Note:</strong> MariaDB accepts the REFERENCES clause in ALTER TABLE and CREATE TABLE statements, but that syntax does nothing. MariaDB simply parses it without returning any error or warning, for compatibility with other DBMS's. However, only the syntax described below creates foreign keys.

Foreign keys are created with [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/) or [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/). The definition must follow this syntax:

```sql
[CONSTRAINT [symbol]] FOREIGN KEY
    [index_name] (index_col_name, ...)
    REFERENCES tbl_name (index_col_name,...)
    [ON DELETE reference_option]
    [ON UPDATE reference_option]

reference_option:
    RESTRICT | CASCADE | SET NULL | NO ACTION | SET DEFAULT
```

The `symbol` clause, if specified, is used in error messages and must be unique in the database.

The columns in the child table must be an index, or the leftmost part of an index. Index prefixes are not supported (thus, [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text/) and [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob/) columns cannot be used as foreign keys). If MariaDB automatically creates an index for the foreign key (because it does not exist and is not explicitly created), its name will be `index_name`.

The referenced columns must be a PRIMARY KEY or a UNIQUE index.

Both the foreign key columns and the referenced columns can be [PERSISTENT](/kb/en/virtual-columns/) columns. However, the ON UPDATE CASCADE, ON UPDATE SET NULL, ON DELETE SET NULL clauses are not allowed in this case.

The foreign key columns and the referenced columns must be of the same type, or similar types. For integer types, the size and sign must also be the same.

The parent and the child table must use the same storage engine, and must not be `TEMPORARY` or partitioned tables. They can be the same table.

## Constraints

If a foreign keys exists, each row in the child table must match a row in the parent table. Multiple child rows can match the same parent row. A child row <em>matches</em> a parent row if all its foreign key values are identical to a parent row's values in the parent table. However, if at least one of the foreign key values is `NULL`, the row has no parents, but it is still allowed.

MariaDB performs certain checks to guarantee that the data integrity is enforced:

- Trying to insert non-matching rows (or update matching rows in a way that makes them non-matching rows) in the child table produces a 1452 error ([SQLSTATE](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-diagnostics/sqlstate/) '23000').
- When a row in the parent table is deleted and at least one child row exists, MariaDB performs an action which depends on the `ON DELETE` clause of the foreign key.
- When a value in the column referenced by a foreign key changes and at least one child row exists, MariaDB performs an action which depends on the `ON UPDATE` clause of the foreign key.
- Trying to drop a table that is referenced by a foreign key produces a 1217 error ([SQLSTATE](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-diagnostics/sqlstate/) '23000').
- A [TRUNCATE TABLE](/sql-statements-structure/sql-statements/table-statements/truncate-table/) against a table containing one or more foreign keys is executed as a [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete/) without WHERE, so that the foreign keys are enforced for each row.

The allowed actions for `ON DELETE` and `ON UPDATE` are:

- `RESTRICT`: The change on the parent table is prevented. The statement terminates with a 1451 error ([SQLSTATE](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-diagnostics/sqlstate/) '2300'). This is the default behavior for both `ON DELETE` and `ON UPDATE`.
- `NO ACTION`: Synonym for `RESTRICT`.
- `CASCADE`: The change is allowed and propagates on the child table. For example, if a parent row is deleted, the child row is also deleted; if a parent row's ID changes, the child row's ID will also change.
- `SET NULL`: The change is allowed, and the child row's foreign key columns are set to `NULL`.
- `SET DEFAULT`: Only worked with PBXT. Similar to `SET NULL`, but the foreign key columns were set to their default values. If default values do not exist, an error is produced.

The delete or update operations triggered by foreign keys do not activate [triggers](/programming-customizing-mariadb/triggers-events/triggers/) and are not counted in the [Com_delete](/kb/en/server-status-variables/#com_delete) and [Com_update](/kb/en/server-status-variables/#com_update) status variables.

Foreign key constraints can be disabled by setting the [foreign_key_checks](/kb/en/server-system-variables/#foreign_key_checks) server system variable to 0. This speeds up the insertion of large quantities of data.

## Metadata

The [Information Schema](/kb/en/information_schema/) <a undefined>REFERENTIAL_CONSTRAINTS</a> table contains information about foreign keys. The individual columns are listed in the <a undefined>KEY_COLUMN_USAGE</a> table.

The InnoDB-specific Information Schema tables also contain information about the InnoDB foreign keys. The foreign key information is stored in the <a undefined>INNODB_SYS_FOREIGN</a>. Data about the individual columns are stored in <a undefined>INNODB_SYS_FOREIGN_COLS</a>.

The most human-readable way to get information about a table's foreign keys sometimes is the [SHOW CREATE TABLE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-table/) statement.

## Limitations

Foreign keys have the following limitations in MariaDB:

- Currently, foreign keys are only supported by InnoDB.
- Cannot be used with views.
- The `SET DEFAULT` action is not supported.
- Foreign keys actions do not activate [triggers](/programming-customizing-mariadb/triggers-events/triggers/).

## Examples

Let's see an example. We will create an `author` table and a `book` table. Both tables have a primary key called `id`. `book` also has a foreign key composed by a field called `author_id`, which refers to the `author` primary key. The foreign key constraint name is optional, but we'll specify it because we want it to appear in error messages: `fk_book_author`.

```sql
CREATE TABLE author (
  id SMALLINT UNSIGNED NOT NULL AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100) NOT NULL
) ENGINE = InnoDB;

CREATE TABLE book (
  id MEDIUMINT UNSIGNED NOT NULL AUTO_INCREMENT PRIMARY KEY,
  title VARCHAR(200) NOT NULL,
  author_id SMALLINT UNSIGNED NOT NULL,
  CONSTRAINT `fk_book_author`
    FOREIGN KEY (author_id) REFERENCES author (id)
    ON DELETE CASCADE
    ON UPDATE RESTRICT
) ENGINE = InnoDB;
```

Now, if we try to insert a book with a non-existing author, we will get an error:

```sql
INSERT INTO book (title, author_id) VALUES ('Necronomicon', 1);
ERROR 1452 (23000): Cannot add or update a child row: a foreign key constraint fails
 (`test`.`book`, CONSTRAINT `fk_book_author` FOREIGN KEY (`author_id`) 
  REFERENCES `author` (`id`) ON DELETE CASCADE)
```

The error is very descriptive.

Now, let's try to properly insert two authors and their books:

```sql
INSERT INTO author (name) VALUES ('Abdul Alhazred');
INSERT INTO book (title, author_id) VALUES ('Necronomicon', LAST_INSERT_ID());

INSERT INTO author (name) VALUES ('H.P. Lovecraft');
INSERT INTO book (title, author_id) VALUES
  ('The call of Cthulhu', LAST_INSERT_ID()),
  ('The colour out of space', LAST_INSERT_ID());
```

It worked!

Now, let's delete the second author. When we created the foreign key, we specified `ON DELETE CASCADE`. This should propagate the deletion, and make the deleted author's books disappear:

```sql
DELETE FROM author WHERE name = 'H.P. Lovecraft';

SELECT * FROM book;
+----+--------------+-----------+
| id | title        | author_id |
+----+--------------+-----------+
|  3 | Necronomicon |         1 |
+----+--------------+-----------+
```

We also specified `ON UPDATE RESTRICT`. This should prevent us from modifying an author's `id` (the column referenced by the foreign key) if a child row exists:

```sql
UPDATE author SET id = 10 WHERE id = 1;
ERROR 1451 (23000): Cannot delete or update a parent row: a foreign key constraint fails 
 (`test`.`book`, CONSTRAINT `fk_book_author` FOREIGN KEY (`author_id`) 
  REFERENCES `author` (`id`) ON DELETE CASCADE)
```

## See also

- [MariaDB: InnoDB foreign key constraint errors](https://blog.mariadb.org/mariadb-innodb-foreign-key-constraint-errors/), a post in the MariaDB blog