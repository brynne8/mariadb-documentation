# Performance Schema setup_objects Table

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

The `setup_objects` table was added in [MariaDB 10.0](/kb/en/what-is-mariadb-100/) (along with many other new
[Performance Schema tables](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/list-of-performance-schema-tables)).

## Description

The `setup_objects` table determines whether objects are monitored by the performance schema or not. By default limited to 100 rows, this can be changed by setting the [performance_schema_setup_objects_size](/kb/en/performance-schema-system-variables/#performance_schema_setup_objects_size) system variable when the server starts.

It contains the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>OBJECT_TYPE</code></td><td>Type of object to instrument, currently only . Currently, only <code>TABLE'</code>, for base table.</td></tr>
<tr><td><code>OBJECT_SCHEMA</code></td><td>Schema containing the object, either the literal or <code>%</code> for any schema.</td></tr>
<tr><td><code>OBJECT_NAME</code></td><td>Name of the instrumented object, either the literal or <code>%</code> for any object.</td></tr>
<tr><td><code>ENABLED</code></td><td>Whether the object's events are instrumented or not. Can be disabled, in which case monitoring is not enabled for those objects.</td></tr>
<tr><td><code>TIMED</code></td><td>Whether the object's events are timed or not. Can be modified.</td></tr>
</tbody></table>

When the Performance Schema looks for matches in the `setup_objects`, there may be more than one row matching, with different `ENABLED` and `TIMED` values. It looks for the most specific matches first, that is, it will first look for the specific database and table name combination, then the specific database, only then falling back to a wildcard for both.

Rows can be added or removed from the table, while for existing rows, only the `TIMED` and `ENABLED` columns can be updated. By default, all tables except those in the `performance_schema`, `information_schema` and `mysql` databases are instrumented.