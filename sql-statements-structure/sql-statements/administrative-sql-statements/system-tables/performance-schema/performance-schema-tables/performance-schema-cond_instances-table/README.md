# Performance Schema cond_instances Table

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

The `cond_instances` table, along with many other new [Performance Schema tables](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/list-of-performance-schema-tables), was added in [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

## Description

The `cond_instances` table lists all conditions while the server is executing. A condition, or instrumented condition object, is an internal code mechanism used for signalling that a specific event has occurred so that any threads waiting for this condition can continue.

The maximum number of conditions stored in the performance schema is determined by the [performance_schema_max_cond_instances](/kb/en/performance-schema-system-variables/#performance_schema_max_cond_instances) system variable.

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>NAME</code></td><td>Client user name for the connection, or <code>NULL</code> if an internal thread.</td></tr>
<tr><td><code>OBJECT_INSTANCE_BEGIN</code></td><td>Address in memory of the instrumented condition.</td></tr>
</tbody></table>