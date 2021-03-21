# DROP VIEW

## Syntax

```sql
DROP VIEW [IF EXISTS]
    view_name [, view_name] ...
    [RESTRICT | CASCADE]
```

## Description

`DROP VIEW` removes one or more [views](/programming-customizing-mariadb/views/). You must have the DROP privilege for
each view. If any of the views named in the argument list do not exist, MariaDB
returns an error indicating by name which non-existing views it was unable to
drop, but it also drops all of the views in the list that do exist.

The `IF EXISTS` clause prevents an error from occurring for views that don't
exist. When this clause is given, a `NOTE` is generated for each non-existent
view. See [SHOW WARNINGS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-warnings/).

`RESTRICT` and `CASCADE`, if given, are parsed and ignored.

It is possible to specify view names as `db_name`.`view_name`. This is useful to delete views from multiple databases with one statement. See [Identifier Qualifiers](/sql-statements-structure/sql-language-structure/identifier-qualifiers/) for details.

The [DROP privilege](/kb/en/grant/#table-privileges) is required to use `DROP TABLE` on non-temporary tables. For temporary tables, no privilege is required, because such tables are only visible for the current session.

If a view references another view, it will be possible to drop the referenced view. However, the other view will reference a view which does not exist any more. Thus, querying it will produce an error similar to the following:

```sql
ERROR 1356 (HY000): View 'db_name.view_name' references invalid table(s) or 
column(s) or function(s) or definer/invoker of view lack rights to use them
```

This problem is reported in the output of [CHECK TABLE](/sql-statements-structure/sql-statements/table-statements/check-table/).

Note that it is not necessary to use `DROP VIEW` to replace an existing view, because [CREATE VIEW](/programming-customizing-mariadb/views/create-view/) has an `OR REPLACE` clause.

## Examples

```sql
DROP VIEW v,v2;
```

Given views `v` and `v2`, but no view `v3`

```sql
DROP VIEW v,v2,v3;
ERROR 1051 (42S02): Unknown table 'v3'
```

```sql
DROP VIEW IF EXISTS v,v2,v3;
Query OK, 0 rows affected, 1 warning (0.01 sec)

SHOW WARNINGS;
+-------+------+-------------------------+
| Level | Code | Message                 |
+-------+------+-------------------------+
| Note  | 1051 | Unknown table 'test.v3' |
+-------+------+-------------------------+
```

## See Also

- [CREATE VIEW](/programming-customizing-mariadb/views/create-view/)
- [ALTER VIEW](/programming-customizing-mariadb/views/alter-view/)
- [SHOW CREATE VIEWS](show-create-views)
- [INFORMATION SCHEMA VIEWS Table](/programming-customizing-mariadb/views/information-schema-views-table/)