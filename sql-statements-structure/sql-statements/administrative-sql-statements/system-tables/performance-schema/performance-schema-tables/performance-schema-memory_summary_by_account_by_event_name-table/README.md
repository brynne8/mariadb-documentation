# Performance Schema memory_summary_by_account_by_event_name Table

##### MariaDB starting with [10.5.2](/kb/en/mariadb-1052-release-notes/)

The memory_summary_by_account_by_event_name table was introduced in [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/).

There are five memory summary tables in the Performance Schema that share a number of fields in common. These include:

- memory_summary_by_account_by_event_name
- [memory_summary_by_host_by_event_name](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-memory_summary_by_host_by_event_name-table)
- [memory_summary_by_thread_by_event_name](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-memory_summary_by_thread_by_event_name-table)
- [memory_summary_by_user_by_event_name](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-memory_summary_by_user_by_event_name-table)
- [memory_global_by_event_name](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-memory_global_by_event_name-table)

The `memory_summary_by_account_by_event_name` table contains memory usage statistics aggregated by account and event.

The table contains the following columns:

<table><tbody><tr><th>Field</th><th>Type</th><th>Null</th><th>Default</th><th>Description</th></tr>
<tr><td>USER</td><td>char(32)</td><td>YES</td><td>NULL</td><td>User portion of the account.</td></tr>
<tr><td>HOST</td><td>char(60)</td><td>YES</td><td>NULL</td><td>Host portion of the account.</td></tr>
<tr><td>EVENT_NAME</td><td>varchar(128)</td><td>NO</td><td>NULL</td><td>Event name.</td></tr>
<tr><td>COUNT_ALLOC</td><td>bigint(20) unsigned</td><td>NO</td><td>NULL</td><td>Total number of allocations to memory.</td></tr>
<tr><td>COUNT_FREE</td><td>bigint(20) unsigned</td><td>NO</td><td>NULL</td><td>Total number of attempts to free the allocated memory.</td></tr>
<tr><td>SUM_NUMBER_OF_BYTES_ALLOC</td><td>bigint(20) unsigned</td><td>NO</td><td>NULL</td><td>Total number of bytes allocated.</td></tr>
<tr><td>SUM_NUMBER_OF_BYTES_FREE</td><td>bigint(20) unsigned</td><td>NO</td><td>NULL</td><td>Total number of bytes freed</td></tr>
<tr><td>LOW_COUNT_USED</td><td>bigint(20)</td><td>NO</td><td>NULL</td><td>Lowest number of allocated blocks (lowest value of CURRENT_COUNT_USED).</td></tr>
<tr><td>CURRENT_COUNT_USED</td><td>bigint(20)</td><td>NO</td><td>NULL</td><td>Currently allocated blocks that have not been freed (COUNT_ALLOC minus COUNT_FREE).</td></tr>
<tr><td>HIGH_COUNT_USED</td><td>bigint(20)</td><td>NO</td><td>NULL</td><td>Highest number of allocated blocks (highest value of CURRENT_COUNT_USED).</td></tr>
<tr><td>LOW_NUMBER_OF_BYTES_USED</td><td>bigint(20)</td><td>NO</td><td>NULL</td><td>Lowest number of bytes used.</td></tr>
<tr><td>CURRENT_NUMBER_OF_BYTES_USED</td><td>bigint(20)</td><td>NO</td><td>NULL</td><td>Current number of bytes used (total allocated minus total freed).</td></tr>
<tr><td>HIGH_NUMBER_OF_BYTES_USED</td><td>bigint(20)</td><td>NO</td><td>NULL</td><td>Highest number of bytes used.</td></tr>
</tbody></table>