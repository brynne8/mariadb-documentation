# Event Scheduler Thread States

This article documents thread states that are related to [event](/programming-customizing-mariadb/triggers-events/event-scheduler/events) scheduling and execution. These include the Event Scheduler thread, threads that terminate the Event Scheduler, and threads for executing events.

These correspond to the `STATE` values listed by the [SHOW PROCESSLIST](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-processlist) statement or in the [Information Schema PROCESSLIST Table](/kb/en/information-schema-processlist-table/) as well as the `PROCESSLIST_STATE` value listed in the [Performance Schema threads Table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-threads-table)

<table><tbody><tr><th>Value</th><th>Description</th></tr>
<tr><td>Clearing</td><td>Thread is terminating.</td></tr>
<tr><td>Initialized</td><td>Thread has be initialized.</td></tr>
<tr><td>Waiting for next activation</td><td>The event queue contains items, but the next activation is at some time in the future.</td></tr>
<tr><td>Waiting for scheduler to stop</td><td>Waiting for the event scheduler to stop after issuing <code>SET GLOBAL event_scheduler=OFF</code>.</td></tr>
<tr><td>Waiting on empty queue</td><td>Sleeping, as the event scheduler's queue is empty.</td></tr>
</tbody></table>