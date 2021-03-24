# Information Schema INNODB_SYS_SEMAPHORE_WAITS Table

##### MariaDB starting with [10.1.3](/kb/en/mariadb-1013-release-notes/)

The [Information Schema](/kb/en/information_schema/) INNODB_SYS_SEMAPHORE_WAITS table is meant to contain information about current semaphore waits. At present it is not correctly populated. See [MDEV-21330](https://jira.mariadb.org/browse/MDEV-21330).

The [PROCESS privilege](/kb/en/grant/#process) is required to view the table.

It contains the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td>THREAD_ID</td><td>Thread id waiting for semaphore</td></tr>
<tr><td>OBJECT_NAME</td><td>Semaphore name</td></tr>
<tr><td>FILE</td><td>File name where semaphore was requested</td></tr>
<tr><td>LINE</td><td>Line number on above file</td></tr>
<tr><td>WAIT_TIME</td><td>Wait time</td></tr>
<tr><td>WAIT_OBJECT</td><td></td></tr>
<tr><td>WAIT_TYPE</td><td>Object type (mutex, rw-lock)</td></tr>
<tr><td>HOLDER_THREAD_ID</td><td>Holder thread id</td></tr>
<tr><td>HOLDER_FILE</td><td>File name where semaphore was acquired</td></tr>
<tr><td>HOLDER_LINE</td><td>Line number for above</td></tr>
<tr><td>CREATED_FILE</td><td>Creation file name</td></tr>
<tr><td>CREATED_LINE</td><td>Line number for above</td></tr>
<tr><td>WRITER_THREAD</td><td>Last write request thread id</td></tr>
<tr><td>RESERVATION_MODE</td><td>Reservation mode (shared, exclusive)</td></tr>
<tr><td>READERS</td><td>Number of readers if only shared mode</td></tr>
<tr><td>WAITERS_FLAG</td><td>Flags</td></tr>
<tr><td>LOCK_WORD</td><td>Lock word (for developers)</td></tr>
<tr><td>LAST_READER_FILE</td><td>Removed</td></tr>
<tr><td>LAST_READER_LINE</td><td>Removed</td></tr>
<tr><td>LAST_WRITER_FILE</td><td>Last writer file name</td></tr>
<tr><td>LAST_WRITER_LINE</td><td>Above line number</td></tr>
<tr><td>OS_WAIT_COUNT</td><td>Wait count</td></tr>
</tbody></table>