# MariaDB Audit Plugin - Location and Rotation of Logs

Logs can be written to a separate file or to the system logs. If you prefer to have the logging separated from other system information, the value of the variable, [server_audit_output_type](/kb/en/mariadb-audit-plugin-system-variables/#server_audit_output_type) should be set to `file`. Incidentally, `file` is the only option on Windows systems.

You can force a rotation by enabling the [server_audit_file_rotate_now](/kb/en/mariadb-audit-plugin-system-variables/#server_audit_file_rotate_now) variable like so:

```sql
SET GLOBAL server_audit_file_rotate_now = ON;
```

### Separate log files

In addition to setting [server_audit_output_type](/kb/en/mariadb-audit-plugin-system-variables/#server_audit_output_type), you will have to provide the file path and name of the audit file. This is set in the variable, [server_audit_file_path](/kb/en/mariadb-audit-plugin-system-variables/#server_audit_file_path). You can set the file size limit of the log file with the variable, [server_audit_file_rotate_size](/kb/en/mariadb-audit-plugin-system-variables/#server_audit_file_rotate_size).

So, if rotation is on and the log file has reached the size limit you set, a copy is created with a consecutive number as extension, the original file will be truncated to be used for the auditing again. To limit the number of log files created, set the variable, [server_audit_file_rotations](/kb/en/mariadb-audit-plugin-system-variables/#server_audit_file_rotations). You can force log file rotation by setting the variable, [server_audit_file_rotate_now](/kb/en/mariadb-audit-plugin-system-variables/#server_audit_file_rotate_now) to a value of `ON`. When the number of files permitted is reached, the oldest file will be overwritten. Below is an example of how the variables described above might be set in a server's configuration files:

```sql
[mysqld]
...
server_audit_file_rotate_now=ON 
server_audit_file_rotate_size=1000000 
server_audit_file_rotations=5
...
```

### System logs

For security reasons, it's better sometimes to use the system logs instead of a local file owned by the `mysql` user. To do this, the value of [server_audit_output_type](/kb/en/mariadb-audit-plugin-system-variables/#server_audit_output_type) needs to be set to `syslog`. Advanced configurations, such as using a remote `syslogd` service, are part of the `syslogd` configuration.

The variables, [server_audit_syslog_ident](/kb/en/mariadb-audit-plugin-system-variables/#server_audit_syslog_ident) and [server_audit_syslog_info](/kb/en/mariadb-audit-plugin-system-variables/#server_audit_syslog_info) can be used to identify a system log entry made by the audit plugin. If a remote `syslogd` service is used for several MariaDB servers, these same variables are also used to identify the MariaDB server.

Below is an example of a system log entry taken from a server which had [server_audit_syslog_ident](/kb/en/mariadb-audit-plugin-system-variables/#server_audit_syslog_ident) set to the default value of `mysql­-server_auditing`, and [server_audit_syslog_info](/kb/en/mariadb-audit-plugin-system-variables/#server_audit_syslog_info) set to `&lt;prod1&gt;`.

```sql
Aug 717:19:58localhostmysql-­server_auditing: 
<prod1> localhost.localdomain,root,localhost,1,7, 
QUERY, mysql, 'SELECT * FROM user',0
```

Although the default values for [server_audit_syslog_facility](/kb/en/mariadb-audit-plugin-system-variables/#server_audit_syslog_facility) and [server_audit_syslog_priority](/kb/en/mariadb-audit-plugin-system-variables/#server_audit_syslog_priority) should be sufficient in most cases, they can be changed based on the definition in `syslog.h` for the functions `openlog()` and `syslog()`.