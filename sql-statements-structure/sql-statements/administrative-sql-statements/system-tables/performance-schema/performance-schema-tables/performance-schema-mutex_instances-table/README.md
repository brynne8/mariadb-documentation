# Performance Schema mutex_instances Table

## Description

The `mutex_instances` table lists all mutexes that the Performance Schema seeing while the server is executing.

A mutex is a code mechanism for ensuring that threads can only access resources one at a time. A second thread attempting to access a resource will find it protected by a mutex, and will wait for it to be unlocked.

The [performance_schema_max_mutex_instances](/kb/en/performance-schema-system-variables/#performance_schema_max_mutex_instances) system variable specifies the maximum number of instrumented mutex instances.

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>NAME</code></td><td>Instrument name associated with the mutex.</td></tr>
<tr><td><code>OBJECT_INSTANCE_BEGIN</code></td><td>Memory address of the instrumented mutex.</td></tr>
<tr><td><code>LOCKED_BY_THREAD_ID</code></td><td>The <code>THREAD_ID</code> of the locking thread if a thread has a mutex locked, otherwise <code>NULL</code>.</td></tr>
</tbody></table>