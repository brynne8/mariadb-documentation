# Performance Schema rwlock_instances Table

The `rwlock_instances` table lists all read write lock (rwlock) instances that the Performance Schema sees while the server is executing. A  read write is a mechanism for ensuring threads can either share access to common resources, or have exclusive access.

The [performance_schema_max_rwlock_instances](/kb/en/performance-schema-system-variables/#performance_schema_max_rwlock_instances) system variable specifies the maximum number of instrumented rwlock objects.

The `rwlock_instances` table contains the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>NAME</code></td><td>Instrument name associated with the read write lock</td></tr>
<tr><td><code>OBJECT_INSTANCE_BEGIN</code></td><td>Address in memory of the instrumented lock</td></tr>
<tr><td><code>WRITE_LOCKED_BY_THREAD_ID</code></td><td><code>THREAD_ID</code> of the locking thread if locked in write (exclusive) mode, otherwise <code>NULL</code>.</td></tr>
<tr><td><code>READ_LOCKED_BY_COUNT</code></td><td>Count of current read locks held</td></tr>
</tbody></table>