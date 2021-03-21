# Information Schema EVENTS Table

The [Information Schema](/kb/en/information_schema/) `EVENTS` table stores information about [Events](/kb/en/stored-programs-and-views-events/) on the server.

It contains the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>EVENT_CATALOG</code></td><td>Always <code>def</code>.</td></tr>
<tr><td><code>EVENT_SCHEMA</code></td><td>Database where the event was defined.</td></tr>
<tr><td><code>EVENT_NAME</code></td><td>Event name.</td></tr>
<tr><td><code>DEFINER</code></td><td>Event definer.</td></tr>
<tr><td><code>TIME_ZONE</code></td><td>Time zone used for the event's scheduling and execution, by default <code>SYSTEM</code>.</td></tr>
<tr><td><code>EVENT_BODY</code></td><td><code>SQL</code>.</td></tr>
<tr><td><code>EVENT_DEFINITION</code></td><td>The SQL defining the event.</td></tr>
<tr><td><code>EVENT_TYPE</code></td><td>Either <code>ONE TIME</code> or <code>RECURRING</code>.</td></tr>
<tr><td><code>EXECUTE_AT</code></td><td><code><a href="/kb/en/datetime/">DATETIME</a></code> when the event is set to execute, or <code>NULL</code> if recurring.</td></tr>
<tr><td><code>INTERVAL_VALUE</code></td><td>Numeric interval between event executions for a recurring event, or <code>NULL</code> if not recurring.</td></tr>
<tr><td><code>INTERVAL_FIELD</code></td><td>Interval unit (e.g., <code>HOUR</code>)</td></tr>
<tr><td><code>SQL_MODE</code></td><td>The <code><a href="/kb/en/sql-mode/">SQL_MODE</a></code> at the time the event was created.</td></tr>
<tr><td><code>STARTS</code></td><td>Start  <code><a href="/kb/en/datetime/">DATETIME</a></code> for a recurring event, <code>NULL</code> if not defined or not recurring.</td></tr>
<tr><td><code>ENDS</code></td><td>End  <code><a href="/kb/en/datetime/">DATETIME</a></code> for a recurring event, <code>NULL</code> if not defined or not recurring.</td></tr>
<tr><td><code>STATUS</code></td><td>One of <code>ENABLED</code>, <code>DISABLED</code> or /<code>SLAVESIDE_DISABLED</code>.</td></tr>
<tr><td><code>ON_COMPLETION</code></td><td>The <code>ON COMPLETION</code> clause, either <code>PRESERVE</code> or <code>NOT PRESERVE</code> .</td></tr>
<tr><td><code>CREATED</code></td><td>When the event was created.</td></tr>
<tr><td><code>LAST_ALTERED</code></td><td>When the event was last changed.</td></tr>
<tr><td><code>LAST_EXECUTED</code></td><td>When the event was last run.</td></tr>
<tr><td><code>EVENT_COMMENT</code></td><td>The comment provided in the <code><a href="/kb/en/create-event/">CREATE EVENT</a></code> statement, or an empty string if none.</td></tr>
<tr><td><code>ORIGINATOR</code></td><td>MariaDB server ID on which the event was created.</td></tr>
<tr><td><code>CHARACTER_SET_CLIENT</code></td><td><code><a href="/kb/en/server-system-variables/#character_set_client">character_set_client</a></code> system variable session value at the time the event was created.</td></tr>
<tr><td><code>COLLATION_CONNECTION</code></td><td><a href="/kb/en/server-system-variables/#collation_connection">collation_connection</a> system variable session value at the time the event was created.</td></tr>
<tr><td><code>DATABASE_COLLATION</code></td><td>Database <a href="/kb/en/data-types-character-sets-and-collations/">collation</a> with which the event is linked.</td></tr>
</tbody></table>

The [SHOW EVENTS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-events) and [SHOW CREATE EVENT](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-event) statements provide similar information.