# ALTER VIEW

## Syntax

```sql
ALTER
    [ALGORITHM = {UNDEFINED | MERGE | TEMPTABLE}]
    [DEFINER = { user | CURRENT_USER }]
    [SQL SECURITY { DEFINER | INVOKER }]
    VIEW view_name [(column_list)]
    AS select_statement
    [WITH [CASCADED | LOCAL] CHECK OPTION]
```

## Description

This statement changes the definition of a [view](/programming-customizing-mariadb/views), which must exist. The
syntax is similar to that for [CREATE VIEW](/programming-customizing-mariadb/views/create-view) and the effect is the same
as for `CREATE OR REPLACE VIEW` if the view exists. This statement
requires the `CREATE VIEW` and `DROP` [privileges](/kb/en/grant/#table-privileges) for the view, and some
privilege for each column referred to in the `SELECT` statement. As of
[MariaDB 5.1.23](/kb/en/mariadb-5123-release-notes/), `ALTER VIEW` is allowed only to the definer or users with
the <a undefined>SUPER</a> privilege.

## Example

```sql
ALTER VIEW v AS SELECT a, a*3 AS a2 FROM t;
```

## See Also

- [CREATE VIEW](/programming-customizing-mariadb/views/create-view)
- [DROP VIEW](/programming-customizing-mariadb/views/drop-view)
- [SHOW CREATE VIEW](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-view)
- [INFORMATION SCHEMA VIEWS Table](/programming-customizing-mariadb/views/information-schema-views-table)