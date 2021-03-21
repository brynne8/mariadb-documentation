# Information Schema TRIGGERS Table

The [Information Schema](/kb/en/information_schema/) `TRIGGERS` table contains information about [triggers](/programming-customizing-mariadb/triggers-events/triggers).

It has the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>TRIGGER_CATALOG</code></td><td>Always <code>def</code>.</td></tr>
<tr><td><code>TRIGGER_SCHEMA</code></td><td>Database name in which the trigger occurs.</td></tr>
<tr><td><code>TRIGGER_NAME</code></td><td>Name of the trigger.</td></tr>
<tr><td><code>EVENT_MANIPULATION</code></td><td>The event that activates the trigger. One of <code>INSERT</code>, <code>UPDATE</code> or <code>'DELETE</code>.</td></tr>
<tr><td><code>EVENT_OBJECT_CATALOG</code></td><td>Always <code>def</code>.</td></tr>
<tr><td><code>EVENT_OBJECT_SCHEMA</code></td><td>Database name on which the trigger acts.</td></tr>
<tr><td><code>EVENT_OBJECT_TABLE</code></td><td>Table name on which the trigger acts.</td></tr>
<tr><td><code>ACTION_ORDER</code></td><td>Indicates the order that the action will be performed in (of the list of a table's triggers with identical <code>EVENT_MANIPULATION</code> and <code>ACTION_TIMING</code> values). Before <a href="/kb/en/mariadb-1023-release-notes/">MariaDB 10.2.3</a> introduced the <a href="/kb/en/create-trigger/#followsprecedes-other_trigger_name">FOLLOWS and PRECEDES clauses</a>, always <code>0</code></td></tr>
<tr><td><code>ACTION_CONDITION</code></td><td><code>NULL</code></td></tr>
<tr><td><code>ACTION_STATEMENT</code></td><td>Trigger body, UTF-8 encoded.</td></tr>
<tr><td><code>ACTION_ORIENTATION</code></td><td>Always <code>ROW</code>.</td></tr>
<tr><td><code>ACTION_TIMING</code></td><td>Whether the trigger acts <code>BEFORE</code> or <code>AFTER</code> the event that triggers it.</td></tr>
<tr><td><code>ACTION_REFERENCE_OLD_TABLE</code></td><td>Always <code>NULL</code>.</td></tr>
<tr><td><code>ACTION_REFERENCE_NEW_TABLE</code></td><td>Always <code>NULL</code>.</td></tr>
<tr><td><code>ACTION_REFERENCE_OLD_ROW</code></td><td>Always <code>OLD</code>.</td></tr>
<tr><td><code>ACTION_REFERENCE_NEW_ROW</code></td><td>Always <code>NEW</code>.</td></tr>
<tr><td><code>CREATED</code></td><td>Always <code>NULL</code>.</td></tr>
<tr><td><code>SQL_MODE</code></td><td>The <code><a href="/kb/en/sql-mode/">SQL_MODE</a></code> when the trigger was created, and which it uses for execution.</td></tr>
<tr><td><code>DEFINER</code></td><td>The account that created the trigger, in the form <code>user_name@host_name</code></td></tr>
<tr><td><code>CHARACTER_SET_CLIENT</code></td><td>The client <a href="/kb/en/data-types-character-sets-and-collations/">character set</a> when the trigger was created, from the session value of the <code><a href="/kb/en/server-system-variables/#character_set_client">character_set_client</a></code> system variable.</td></tr>
<tr><td><code>COLLATION_CONNECTION</code></td><td>The client <a href="/kb/en/data-types-character-sets-and-collations/">collation</a> when the trigger was created, from the session value of the <a href="/kb/en/server-system-variables/#collation_connection">collation_connection</a> system variable.</td></tr>
<tr><td><code>DATABASE_COLLATION</code></td><td><a href="/kb/en/data-types-character-sets-and-collations/">Collation</a> of the associated database.</td></tr>
</tbody></table>

Queries to the `TRIGGERS` table will return information only for databases and tables for which you have the `TRIGGER` [privilege](/kb/en/grant/#table-privileges). Similar information is returned by the [SHOW TRIGGERS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-triggers) statement.

## See also

- [Trigger Overview](/programming-customizing-mariadb/triggers-events/triggers/trigger-overview)
- [CREATE TRIGGER](/programming-customizing-mariadb/triggers-events/triggers/create-trigger)
- [DROP TRIGGER](/sql-statements-structure/sql-statements/data-definition/drop/drop-trigger)
- [SHOW TRIGGERS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-triggers)
- [SHOW CREATE TRIGGER](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-trigger)
- [Trigger Limitations](/programming-customizing-mariadb/triggers-events/triggers/trigger-limitations)