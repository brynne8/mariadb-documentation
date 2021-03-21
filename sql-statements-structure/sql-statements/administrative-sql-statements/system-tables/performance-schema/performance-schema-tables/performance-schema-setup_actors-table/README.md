# Performance Schema setup_actors Table

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

The `setup_actors` table was added in [MariaDB 10.0](/kb/en/what-is-mariadb-100/) (along with many other new
[Performance Schema tables](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/list-of-performance-schema-tables)).

The `setup_actors` table contains information for determining whether
monitoring should be enabled for new client connection threads.

The default size is 100 rows, which can be changed by modifying the
<a undefined>performance_schema_setup_actors_size</a>
system variable at server startup.

If a row in the table matches a new foreground thread's client and host, the
matching `INSTRUMENTED` column in the
[threads](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-threads-table) table is set to either `YES` or
`NO`, which allows selective application of instrumenting by host, by user,
or combination thereof.

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>HOST</code></td><td>Host name, either a literal, or the <code>%</code> wildcard representing any host.</td></tr>
<tr><td><code>USER</code></td><td>User name, either a literal or the <code>%</code> wildcard representing any name.</td></tr>
<tr><td><code>ROLE</code></td><td>Unused</td></tr>
</tbody></table>

Initially, any user and host is matched:

```sql
SELECT * FROM performance_schema.setup_actors;
+------+------+------+
| HOST | USER | ROLE |
+------+------+------+
| %    | %    | %    |
+------+------+------+
```