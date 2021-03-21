# SHOW OPEN TABLES

## Syntax

```sql
SHOW OPEN TABLES [FROM db_name]
    [LIKE 'pattern' | WHERE expr]
```

## Description

<code class="highlight fixed" style="white-space:pre-wrap">SHOW OPEN TABLES</code> lists the non-<code class="highlight fixed" style="white-space:pre-wrap">TEMPORARY</code>
tables that are currently open in the table cache. See
[http://dev.mysql.com/doc/refman/5.1/en/table-cache.html](http://dev.mysql.com/doc/refman/5.1/en/table-cache.html).

The <code class="highlight fixed" style="white-space:pre-wrap">FROM</code> and <code class="highlight fixed" style="white-space:pre-wrap">LIKE</code> clauses may be used.

The <code class="highlight fixed" style="white-space:pre-wrap">FROM</code>
clause, if present, restricts the tables shown to those present in the
<code class="highlight fixed" style="white-space:pre-wrap">db_name</code> database.

The <code class="highlight fixed" style="white-space:pre-wrap">LIKE</code> clause, if
present on its own, indicates which table names to match. The <code class="highlight fixed" style="white-space:pre-wrap">WHERE</code> and <code class="highlight fixed" style="white-space:pre-wrap">LIKE</code> clauses can be given to select rows using more general conditions, as discussed in [Extended SHOW](/sql-statements-structure/sql-statements/administrative-sql-statements/show/extended-show/).

The following information is returned:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td>Database</td><td>Database name.</td></tr>
<tr><td>Name</td><td>Table name.</td></tr>
<tr><td>In_use</td><td>Number of  table instances being used.</td></tr>
<tr><td>Name_locked</td><td><code>1</code> if the table is name-locked, e.g. if it is being dropped or renamed, otherwise <code>0</code>.</td></tr>
</tbody></table>

Before [MariaDB 5.5](/kb/en/what-is-mariadb-55/), each use of, for example, [LOCK TABLE ... WRITE](/kb/en/lock-tables-and-unlock-tables/) would increment `In_use` for that table. With the implementation of the metadata locking improvements in [MariaDB 5.5](/kb/en/what-is-mariadb-55/), `LOCK TABLE... WRITE` acquires a strong MDL lock, and concurrent connections will wait on this MDL lock, so any subsequent `LOCK TABLE... WRITE` will not increment `In_use`.

## Example

```sql
SHOW OPEN TABLES;
+----------+---------------------------+--------+-------------+
| Database | Table                     | In_use | Name_locked |
+----------+---------------------------+--------+-------------+
...
| test     | xjson                     |      0 |           0 |
| test     | jauthor                   |      0 |           0 |
| test     | locks                     |      1 |           0 |
...
+----------+---------------------------+--------+-------------+
```