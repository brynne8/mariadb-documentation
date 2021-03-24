# Information Schema INNODB_SYS_DATAFILES Table

##### MariaDB until [10.5](/kb/en/what-is-mariadb-105/)

The `INNODB_SYS_DATAFILES` table was added in [MariaDB 10.0.4](/kb/en/mariadb-1004-release-notes/), and removed in [MariaDB 10.6.0](/kb/en/mariadb-1060-release-notes/).

The [Information Schema](/kb/en/information_schema/) `INNODB_SYS_DATAFILES` table contains information about InnoDB datafile paths. The `PROCESS` [privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) is required to view the table.

It contains the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>SPACE</code></td><td>Numeric tablespace. Matches the <a href="/kb/en/information-schema-innodb_sys_tables-table/">INNODB_SYS_TABLES.SPACE</a> value.</td></tr>
<tr><td><code>PATH</code></td><td>Tablespace datafile path.</td></tr>
</tbody></table>

## Example

```sql
SELECT * FROM INNODB_SYS_DATAFILES;
+-------+--------------------------------+
| SPACE | PATH                           |
+-------+--------------------------------+
|    19 | ./test/t2.ibd                  |
|    20 | ./test/t3.ibd                  |
...
|    68 | ./test/animals.ibd             |
|    69 | ./test/animal_count.ibd        |
|    70 | ./test/t.ibd                   |
+-------+--------------------------------+
```