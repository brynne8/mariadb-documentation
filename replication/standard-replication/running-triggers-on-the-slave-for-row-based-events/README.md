# Running Triggers on the Slave for Row-based Events

The terms <em>master</em> and <em>slave</em> have historically been used in replication, but the terms terms <em>primary</em> and <em>replica</em> are now preferred. The old terms are used throughout the documentation, and in MariaDB commands, although [MariaDB 10.5](/kb/en/what-is-mariadb-105/) has begun the process of renaming. The documentation will follow over time. See [MDEV-18777](https://jira.mariadb.org/browse/MDEV-18777) to follow progress on this effort.

##### MariaDB starting with [10.1.1](/kb/en/mariadb-1011-release-notes/)

Starting from [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/), one can force the slave thread to run [triggers](/programming-customizing-mariadb/triggers-events/triggers/) for row-based binlog events.

The setting is controlled by the [slave_run_triggers_for_rbr](/kb/en/replication-and-binary-log-server-system-variables/#slave_run_triggers_for_rbr) global variable. It can be also specified as a command-line option or in my.cnf.

Possible values are:

<table><tbody><tr><th>Value</th><th>Meaning</th></tr>
<tr><td>NO (Default)</td><td>Don't invoke triggers for row-based events</td></tr>
<tr><td>YES</td><td>Invoke triggers for row-based events, don't log their effect into the binary log</td></tr>
<tr><td>LOGGING</td><td>Invoke triggers for row-based events, and log their effect into the binary log</td></tr>
<tr><td>ENFORCE</td><td>From <a href="/kb/en/mariadb-1052-release-notes/">MariaDB 10.5.2</a> only. Triggers will always be run on the slave, even if there are triggers on the master. ENFORCE implies LOGGING.</td></tr>
</tbody></table>

<strong>Note that if you just want to use triggers together with replication, you most likely don't need this option.</strong> Read below for details.

## When to Use slave_run_triggers_for_rbr

### Background

Normally, MariaDB's replication system can replicate trigger actions automatically.

- When one uses statement-based replication, the binary log contains SQL statements. Slave server(s)  execute the SQL statements.  Triggers are run on the master and on each slave, independently.
- When one uses row-based replication, the binary log contains row changes. It will have both the changes made by the statement itself, and the changes made by the triggers that were invoked by the statement. Slave server(s) do not need to run triggers for row changes they are applying.

### Target Usecase

One may want to have a setup where a slave has triggers that are not present on the master (Suppose the slave needs to update summary tables or perform some other ETL-like process).

If one uses statement-based replication, they can just create the required triggers on the slave. The slave will run the statements from the binary log, which will cause the triggers to be invoked.

However, there are cases where you have to use row-based replication. It could be because the master runs non-deterministic statements, or the master could be a node in a Galera cluster. In that case, you would want row-based events to invoke triggers on the slave.  This is what the `slave_run_triggers_for_rbr` option is for. Setting the option to `YES` will cause the SQL slave thread to invoke triggers for row-based events; setting it to `LOGGING` will also cause the changes made by the triggers to be written into the binary log.

The following triggers are invoked:

- Update_row_event runs an UPDATE trigger
- Delete_row_event runs a DELETE trigger
- Write_row_event runs an INSERT trigger. Additionally it runs a DELETE trigger if there was a conflicting row that had to be deleted.

## Preventing Multiple Trigger Invocations

There is a basic protection against triggers being invoked both on the master and slave.
If the master modifies a table that has triggers, it will produce row-based binlog events with the "triggers were invoked for this event" flag. The slave will not invoke any triggers for flagged events.

## See Also

- Task in Jira, [MDEV-5095](https://jira.mariadb.org/browse/MDEV-5095).