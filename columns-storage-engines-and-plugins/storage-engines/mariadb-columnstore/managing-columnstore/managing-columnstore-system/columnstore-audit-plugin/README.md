# ColumnStore Audit Plugin

# Introduction

MariaDB server includes an optional [Audit Plugin](/columns-storage-engines-and-plugins/plugins/mariadb-audit-plugin) that enables logging and tracking of all user access and statements. This is included and can be enabled for ColumnStore

# Installation

To enable the audit plugin for the currently running instance (but no across restarts) run the following as mcsmysql with the default root account:

```sql
INSTALL PLUGIN server_audit 
SONAME 'server_audit.so';
```

To have this persist across restarts edit the ColumnStore my.cnf file (example shown for root install):

```sql
$ vi /usr/local/mariadb/columnstore/mysql/my.cnf
[mysqld]
... 
plugin_load=server_audit=server_audit.so 
```

For more details see the [audit plugin installation guide](/columns-storage-engines-and-plugins/plugins/mariadb-audit-plugin/mariadb-audit-plugin-installation)

# Enabling the audit plugin

To enable audit logging the following global variable must be set to ON:

```sql
SET GLOBAL server_audit_logging=ON;
```

To ensure this persists across restarts edit the ColumnStore my.cnf file (example shown for root install):

```sql
$ vi /usr/local/mariadb/columnstore/mysql/my.cnf
[server]
... 
server_audit_logging=ON
```

This will enable logging to the file /usr/local/mariadb/columnstore/mysql/db/server_audit.log. For example:

```sql
20170914 17:31:24,centos,root,localhost,11,114,QUERY,loans,'SELECT grade, AVG(loan_amnt) avg, FROM loanstats GROUP BY grade ORDER BY grade',0
```

To have the log entries written to syslog the global variable server_audit_output_type should be changed from 'file' to 'syslog'.  In this case the 'syslog_info' entry contains the ColumnStore server instance name, for example:

```sql
Sep 14 17:46:51 centos mysql-server_auditing: columnstore-1 centos,root,localhost,11,117,QUERY,loans,'SELECT grade, AVG(loan_amnt) avg,FROM loanstats GROUP BY grade ORDER BY grade',0
```

For additional configuration and customization options see the [Audit Plugin](/columns-storage-engines-and-plugins/plugins/mariadb-audit-plugin) documentation.