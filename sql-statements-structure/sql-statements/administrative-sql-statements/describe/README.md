# DESCRIBE

## Syntax

```sql
{DESCRIBE | DESC} tbl_name [col_name | wild]
```

## Description

<code class="highlight fixed" style="white-space:pre-wrap">DESCRIBE</code> provides information about the columns in a table.
It is a shortcut for <code class="highlight fixed" style="white-space:pre-wrap">[SHOW COLUMNS FROM](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-columns)</code>.
These statements also display information for [views](/programming-customizing-mariadb/views).

<code class="highlight fixed" style="white-space:pre-wrap">col_name</code> can be a column name, or a string containing the
SQL "<code class="highlight fixed" style="white-space:pre-wrap">%</code>" and "<code class="highlight fixed" style="white-space:pre-wrap">_</code>" wildcard characters to
obtain output only for the columns with names matching the string. There is no
need to enclose the string within quotes unless it contains spaces or other
special characters.

```sql
DESCRIBE city;
+------------+----------+------+-----+---------+----------------+
| Field      | Type     | Null | Key | Default | Extra          |
+------------+----------+------+-----+---------+----------------+
| Id         | int(11)  | NO   | PRI | NULL    | auto_increment |
| Name       | char(35) | YES  |     | NULL    |                |
| Country    | char(3)  | NO   | UNI |         |                |
| District   | char(20) | YES  | MUL |         |                |
| Population | int(11)  | YES  |     | NULL    |                |
+------------+----------+------+-----+---------+----------------+
```

The description for <code class="highlight fixed" style="white-space:pre-wrap">[SHOW COLUMNS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-columns)</code> provides
more information about the output columns.

## See Also

- [SHOW COLUMNS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-columns)
- [INFORMATION_SCHEMA.COLUMNS Table](/kb/en/information-schema-columns-table/)
- [mysqlshow](/clients-utilities/mysqlshow)