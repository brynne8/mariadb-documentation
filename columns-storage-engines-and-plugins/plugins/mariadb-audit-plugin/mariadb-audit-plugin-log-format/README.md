# MariaDB Audit Plugin - Log Format

The audit plugin logs user access to MariaDB and its objects. The audit trail (i.e., audit log) is a set of records, written as a list of fields to a file in a plain‐text format. The fields in the log are separated by commas. The format used for the plugin's own log file is slightly different from the format used if it logs to the system log because it has its own standard format. The general format for the logging to the plugin's own file is defined like the following:

```sql
[timestamp],[serverhost],[username],[host],[connectionid],
[queryid],[operation],[database],[object],[retcode]
```

If the [server_audit_output_type](/kb/en/server_audit-system-variables/#server_audit_output_type) variable is set to `syslog` instead of the default, `file`, the audit log file format will be as follows:

```sql
[timestamp][syslog_host][syslog_ident]:[syslog_info][serverhost],[username],[host],
[connectionid],[queryid],[operation],[database],[object],[retcode]
```

<table><tbody><tr><th>Item logged</th><th>Description</th></tr>
<tr><td>timestamp</td><td>Time at which the event occurred. If syslog is used, the format is defined by <code>syslogd</code>.</td></tr>
<tr><td>syslog_host</td><td>Host from which the syslog entry was received.</td></tr>
<tr><td>syslog_ident</td><td>For identifying a system log entry, including the MariaDB server.</td></tr>
<tr><td>syslog_info</td><td>For providing information for identifying a system log entry.</td></tr>
<tr><td>serverhost</td><td>The MariaDB server host name.</td></tr>
<tr><td>username</td><td>Connected user.</td></tr>
<tr><td>host</td><td>Host from which the user connected.</td></tr>
<tr><td>connectionid</td><td>Connection ID number for the related operation.</td></tr>
<tr><td>queryid</td><td>Query ID number, which can be used for finding the relational table events and related queries. For TABLE events, multiple lines will be added.</td></tr>
<tr><td>operation</td><td>Recorded action type: CONNECT, QUERY, READ, WRITE, CREATE, ALTER, RENAME, DROP.</td></tr>
<tr><td>database</td><td>Active database (as set by <a href="/kb/en/use/">USE</a>).</td></tr>
<tr><td>object</td><td>Executed query for QUERY events, or the table name in the case of TABLE events.</td></tr>
<tr><td>retcode</td><td>Return code of the logged operation.</td></tr>
</tbody></table>

Various events will result in different audit records. Some events will not return a value for some fields (e.g., when the active database is not set when connecting to the server).

Below is a generic example of the output for connect events, with placeholders representing data. These are events in which a user connected, disconnected, or tried unsuccessfully to connect to the server.

```sql
[timestamp],[serverhost],[username],[host],[connectionid],0,CONNECT,[database],,0 
[timestamp],[serverhost],[username],[host],[connectionid],0,DISCONNECT,,,0 
[timestamp],[serverhost],[username],[host],[connectionid],0,FAILED_CONNECT,,,[retcode]
```

Here is the one audit record generated for each query event:

```sql
[timestamp],[serverhost],[username],[host],[connectionid],[queryid],QUERY,[database],[object], [retcode]
```

Below are generic examples of records that are entered in the audit log for each type of table event:

```sql
[timestamp],[serverhost],[username],[host],[connectionid],[queryid],CREATE,[database],[object], 
[timestamp],[serverhost],[username],[host],[connectionid],[queryid],READ,[database],[object], 
[timestamp],[serverhost],[username],[host],[connectionid],[queryid],WRITE,[database],[object], 
[timestamp],[serverhost],[username],[host],[connectionid],[queryid],ALTER,[database],[object], 
[timestamp],[serverhost],[username],[host],[connectionid],[queryid],RENAME,[database], 
[object_old]|[database_new].[object_new], 
[timestamp],[serverhost],[username],[host],[connectionid],[queryid],DROP,[database],[object],
```

Starting in version 1.2.0, passwords are hidden in the log for certain types of queries. They are replaced with asterisks for `GRANT`, `CREATE USER`, `CREATE MASTER`, `CREATE SERVER`, and `ALTER SERVER` statements. Passwords, however, are not replaced for the `PASSWORD()` and `OLD_PASSWORD()` functions when they are used inside other SQL statements (i.e., SET PASSWORD`).`