# SHOW TABLES

## Syntax

```sql
SHOW [FULL] TABLES [FROM db_name]
    [LIKE 'pattern' | WHERE expr]
```

## Description

`SHOW TABLES` lists the non-`TEMPORARY` tables, [sequences](/sql-statements-structure/sequences) and [views](/programming-customizing-mariadb/views) in a given database.

The `LIKE` clause, if present on its own, indicates which table names to match. The `WHERE` and `LIKE` clauses can be given to select rows using more general conditions, as discussed in [Extended SHOW](/sql-statements-structure/sql-statements/administrative-sql-statements/show/extended-show). For example, when searching for tables in the `test` database, the column name for use in the `WHERE` and `LIKE` clauses will be `Tables_in_test`

The `FULL` modifier is supported such that `SHOW FULL TABLES` displays a second output column. Values for the second column. `Table_type`, are `BASE TABLE` for a table, `VIEW` for a [view](/programming-customizing-mariadb/views) and `SEQUENCE` for a [sequence](/sql-statements-structure/sequences).

You can also get this information using:

```sql
mysqlshow db_name
```

See [mysqlshow](/clients-utilities/mysqlshow) for more details.

If you have no privileges for a base table or view, it does not show up in the output from `SHOW TABLES` or `mysqlshow <em>db_name</em>`.

The <a undefined>information_schema.TABLES</a> table, as well as the [SHOW TABLE STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-table-status) statement, provide extended information about tables.

## Examples

```sql
SHOW TABLES;
+----------------------+
| Tables_in_test       |
+----------------------+
| animal_count         |
| animals              |
| are_the_mooses_loose |
| aria_test2           |
| t1                   |
| view1                |
+----------------------+
```

Showing the tables beginning with <em>a</em> only.

```sql
SHOW TABLES WHERE Tables_in_test LIKE 'a%';
+----------------------+
| Tables_in_test       |
+----------------------+
| animal_count         |
| animals              |
| are_the_mooses_loose |
| aria_test2           |
+----------------------+
```

Showing tables and table types:

```sql
SHOW FULL TABLES;
+----------------+------------+
| Tables_in_test | Table_type |
+----------------+------------+
| s1             | SEQUENCE   |
| student        | BASE TABLE |
| v1             | VIEW       |
+----------------+------------+
```

## See Also

- [SHOW TABLE STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-table-status)
- The [information_schema.TABLES](/kb/en/information-schema-tables-table/) table