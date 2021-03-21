# SHOW SLAVE STATUS

## Syntax

```sql
SHOW SLAVE ["connection_name"] STATUS
SHOW REPLICA ["connection_name"] STATUS -- From MariaDB 10.5.1
```

or

```sql
SHOW ALL SLAVES STATUS
SHOW ALL REPLICAS STATUS -- From MariaDB 10.5.1
```

## Description

This statement is to be run on a replica and provides status information on essential parameters of the [replica](/replication/) threads.

This statement requires the [SUPER](/kb/en/grant/#super) privilege, the [REPLICATION_CLIENT](/kb/en/grant/#replication-client) privilege, or, from [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), the [REPLICATION SLAVE ADMIN](/kb/en/grant/#binlog-monitor) privilege, or, from [MariaDB 10.5.9](/kb/en/mariadb-1059-release-notes/), the [REPLICA MONITOR](/kb/en/grant/#replica-monitor) privilege.

### Multi-Source

The `FULL` and `"connection_name"` options allow you to connect to [many primaries at the same time](/replication/standard-replication/multi-source-replication/).

`ALL SLAVES` (or `ALL REPLICAS` from [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/)) gives you a list of all connections to the primary nodes.

The rows will be sorted according to `Connection_name`.

If you specify a `connection_name`, you only get the information about that
connection. If `connection_name` is not used, then the name set by `default_master_connection` is used. If the connection name doesn't exist you will get an error:
`There is no master connection for 'xxx'`.

### Column Descriptions

<table><tbody><tr><th>Name</th><th>Description</th><th>Added</th></tr>
<tr><td>Connection_name</td><td>Name of the primary connection. Returned with <code>SHOW ALL SLAVES STATUS</code> (or <code>SHOW ALL REPLICAS STATUS</code> from <a href="/kb/en/mariadb-1051-release-notes/">MariaDB 10.5.1</a>) only.</td><td></td></tr>
<tr><td>Slave_SQL_State</td><td>State of SQL thread. Returned with <code>SHOW ALL SLAVES STATUS</code> (or <code>SHOW ALL REPLICAS STATUS</code> from <a href="/kb/en/mariadb-1051-release-notes/">MariaDB 10.5.1</a>) only. See <a href="/kb/en/slave-sql-thread-states/">Slave SQL Thread States</a>.</td><td></td></tr>
<tr><td>Slave_IO_State</td><td>State of I/O thread. See <a href="/kb/en/slave-io-thread-states/">Slave I/O Thread States</a>.</td><td></td></tr>
<tr><td>Master_host</td><td>Master host that the replica is connected to.</td><td></td></tr>
<tr><td>Master_user</td><td>Account user name being used to connect to the primary.</td><td></td></tr>
<tr><td>Master_port</td><td>The port being used to connect to the primary.</td><td></td></tr>
<tr><td>Connect_Retry</td><td>Time in seconds between retries to connect. The default is 60. The <a href="/kb/en/change-master-to/">CHANGE MASTER TO</a> statement can set this. The <a href="/kb/en/mysqld-options/#-master-retry-count">master-retry-count</a> option determines the maximum number of reconnection attempts.</td><td></td></tr>
<tr><td>Master_Log_File</td><td>Name of the primary <a href="/kb/en/binary-log/">binary log</a> file that the I/O thread is currently reading from.</td><td></td></tr>
<tr><td>Read_Master_Log_Pos</td><td>Position up to which the I/O thread has read in the current primary <a href="/kb/en/binary-log/">binary log</a> file.</td><td></td></tr>
<tr><td>Relay_Log_File</td><td>Name of the relay log file that the SQL thread is currently processing.</td><td></td></tr>
<tr><td>Relay_Log_Pos</td><td>Position up to which the SQL thread has finished processing in the current relay log file.</td><td></td></tr>
<tr><td>Relay_Master_Log_File</td><td>Name of the primary <a href="/kb/en/binary-log/">binary log</a> file that contains the most recent event executed by the SQL thread.</td><td></td></tr>
<tr><td>Slave_IO_Running</td><td>Whether the replica I/O thread is running and connected (<code>Yes</code>), running but not connected to a primary (<code>Connecting</code>) or not running (<code>No</code>).</td><td></td></tr>
<tr><td>Slave_SQL_Running</td><td>Whether or not the SQL thread is running.</td><td></td></tr>
<tr><td>Replicate_Do_DB</td><td>Databases specified for replicating with the <code><a href="/kb/en/replication-and-binary-log-server-system-variables/#replicate_do_db">replicate_do_db</a></code> option.</td><td></td></tr>
<tr><td>Replicate_Ignore_DB</td><td>Databases specified for ignoring with the <code><a href="/kb/en/replication-and-binary-log-server-system-variables/#replicate_ignore_db">replicate_ignore_db</a></code> option.</td><td></td></tr>
<tr><td>Replicate_Do_Table</td><td>Tables specified for replicating with the <code><a href="/kb/en/replication-and-binary-log-server-system-variables/#replicate_do_table">replicate_do_table</a></code> option.</td><td></td></tr>
<tr><td>Replicate_Ignore_Table</td><td>Tables specified for ignoring with the <code><a href="/kb/en/replication-and-binary-log-server-system-variables/#replicate_ignore_table">replicate_ignore_table</a></code> option.</td><td></td></tr>
<tr><td>Replicate_Wild_Do_Table</td><td>Tables specified for replicating with the <code><a href="/kb/en/replication-and-binary-log-server-system-variables/#replicate_wild_do_table">replicate_wild_do_table</a></code> option.</td><td></td></tr>
<tr><td>Replicate_Wild_Ignore_Table</td><td>Tables specified for ignoring with the <code><a href="/kb/en/replication-and-binary-log-server-system-variables/#replicate_wild_ignore_table">replicate_wild_ignore_table</a></code> option.</td><td></td></tr>
<tr><td>Last_Errno</td><td>Alias for <code>Last_SQL_Errno</code> (see below)</td><td></td></tr>
<tr><td>Last Error</td><td>Alias for <code>Last_SQL_Error</code> (see below)</td><td></td></tr>
<tr><td>Skip_Counter</td><td>Number of events that a replica skips from the master, as recorded in the <a href="/kb/en/replication-and-binary-log-server-system-variables/#sql_slave_skip_counter">sql_slave_skip_counter</a> system variable.</td><td></td></tr>
<tr><td>Exec_Master_Log_Pos</td><td>Position up to which the SQL thread has processed in the current master <a href="/kb/en/binary-log/">binary log</a> file. Can be used to start a new replica from a current replica with the <a href="/kb/en/change-master-to/">CHANGE MASTER TO ... MASTER_LOG_POS</a> option.</td><td></td></tr>
<tr><td>Relay_Log_Space</td><td>Total size of all relay log files combined.</td><td></td></tr>
<tr><td>Until_Condition</td><td></td><td></td></tr>
<tr><td>Until_Log_File</td><td>The <code>MASTER_LOG_FILE</code> value of the <a href="/kb/en/start-slave/">START SLAVE UNTIL</a> condition.</td><td></td></tr>
<tr><td>Until_Log_Pos</td><td>The <code>MASTER_LOG_POS</code> value of the <a href="/kb/en/start-slave/">START SLAVE UNTIL</a> condition.</td><td></td></tr>
<tr><td>Master_SSL_Allowed</td><td>Whether an SSL connection is permitted (<code>Yes</code>), not permitted (<code>No</code>) or permitted but without the replica having SSL support enabled (<code>Ignored</code>)</td><td></td></tr>
<tr><td>Master_SSL_CA_File</td><td>The <code>MASTER_SSL_CA</code> option of the <code><a href="/kb/en/change-master-to/">CHANGE MASTER TO</a></code> statement.</td><td></td></tr>
<tr><td>Master_SSL_CA_Path</td><td>The <code>MASTER_SSL_CAPATH</code> option of the <code><a href="/kb/en/change-master-to/">CHANGE MASTER TO</a></code> statement.</td><td></td></tr>
<tr><td>Master_SSL_Cert</td><td>The <code>MASTER_SSL_CERT</code> option of the <code><a href="/kb/en/change-master-to/">CHANGE MASTER TO</a></code> statement.</td><td></td></tr>
<tr><td>Master_SSL_Cipher</td><td>The <code>MASTER_SSL_CIPHER</code> option of the <code><a href="/kb/en/change-master-to/">CHANGE MASTER TO</a></code> statement.</td><td></td></tr>
<tr><td>Master_SSL_Key</td><td>The <code>MASTER_SSL_KEY</code> option of the <code><a href="/kb/en/change-master-to/">CHANGE MASTER TO</a></code> statement.</td><td></td></tr>
<tr><td>Seconds_Behind_Master</td><td>Difference between the timestamp logged on the master for the event that the replica is currently processing, and the current timestamp on the replica. Zero if the replica is not currently processing an event. From <a href="/kb/en/mariadb-10023-release-notes/">MariaDB 10.0.23</a> and <a href="/kb/en/mariadb-1019-release-notes/">MariaDB 10.1.9</a>, with <a href="/kb/en/parallel-replication/">parallel replication</a>, <code>seconds_behind_master</code> is updated only after transactions commit.</td><td></td></tr>
<tr><td>Master_SSL_Verify_Server_Cert</td><td>The <code>MASTER_SSL_VERIFY_SERVER_CERT</code> option of the <code><a href="/kb/en/change-master-to/">CHANGE MASTER TO</a></code> statement.</td><td></td></tr>
<tr><td>Last_IO_Errno</td><td><a href="/kb/en/mariadb-error-codes/">Error code</a> of the most recent error that caused the I/O thread to stop (also recorded in the replica's error log). <code>0</code> means no error. <a href="/kb/en/reset-slave/">RESET SLAVE</a> or <a href="/kb/en/reset-master/">RESET MASTER</a> will reset this value.</td><td></td></tr>
<tr><td>Last_IO_Error</td><td><a href="/kb/en/mariadb-error-codes/">Error message</a> of the most recent error that caused the I/O thread to stop (also recorded in the replica's error log). An empty string means no error. <a href="/kb/en/reset-slave/">RESET SLAVE</a> or <a href="/kb/en/reset-master/">RESET MASTER</a> will reset this value.</td><td></td></tr>
<tr><td>Last_SQL_Errno</td><td><a href="/kb/en/mariadb-error-codes/">Error code</a> of the most recent error that caused the SQL thread to stop (also recorded in the replica's error log). <code>0</code> means no error. <a href="/kb/en/reset-slave/">RESET SLAVE</a> or <a href="/kb/en/reset-master/">RESET MASTER</a> will reset this value.</td><td></td></tr>
<tr><td>Last_SQL_Error</td><td><a href="/kb/en/mariadb-error-codes/">Error message</a> of the most recent error that caused the SQL thread to stop (also recorded in the replica's error log). An empty string means no error. <a href="/kb/en/reset-slave/">RESET SLAVE</a> or <a href="/kb/en/reset-master/">RESET MASTER</a> will reset this value.</td><td></td></tr>
<tr><td>Replicate_Ignore_Server_Ids</td><td>List of <a href="/kb/en/replication-and-binary-log-server-system-variables/#server_id">server_ids</a> that are currently being ignored for replication purposes, or an empty string for none, as specified in the <code>IGNORE_SERVER_IDS</code> option of the <code><a href="/kb/en/change-master-to/#ignore_server_ids">CHANGE MASTER TO</a></code> statement.</td><td></td></tr>
<tr><td>Master_Server_Id</td><td>The master's <a href="/kb/en/replication-and-binary-log-server-system-variables/#server_id">server_id</a> value.</td><td></td></tr>
<tr><td>Master_SSL_Crl</td><td>The <code>MASTER_SSL_CRL</code> option of the <code><a href="/kb/en/change-master-to/">CHANGE MASTER TO</a></code> statement.</td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td>Master_SSL_Crlpath</td><td>The <code>MASTER_SSL_CRLPATH</code> option of the <code><a href="/kb/en/change-master-to/">CHANGE MASTER TO</a></code> statement.</td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td>Using_Gtid</td><td>Whether or not <a href="/kb/en/global-transaction-id/">global transaction ID's</a> are being used for replication (can be <code>No</code>, <code>Slave_Pos</code>, or <code>Current_Pos</code>).</td><td><a href="/kb/en/mariadb-1002-release-notes/">MariaDB 10.0.2</a></td></tr>
<tr><td>Gtid_IO_Pos</td><td>Current <a href="/kb/en/global-transaction-id/">global transaction ID</a> value.</td><td><a href="/kb/en/mariadb-1002-release-notes/">MariaDB 10.0.2</a></td></tr>
<tr><td>Retried_transactions</td><td>Number of retried transactions for this connection. Returned with <code>SHOW ALL SLAVES STATUS</code> only.</td><td></td></tr>
<tr><td>Max_relay_log_size</td><td>Max <a href="/kb/en/relay-log/">relay log</a> size for this connection. Returned with <code>SHOW ALL SLAVES STATUS</code> only.</td><td></td></tr>
<tr><td>Executed_log_entries</td><td>How many log entries the replica has executed. Returned with <code>SHOW ALL SLAVES STATUS</code> only.</td><td></td></tr>
<tr><td>Slave_received_heartbeats</td><td>How many heartbeats we have got from the master. Returned with <code>SHOW ALL SLAVES STATUS</code> only.</td><td></td></tr>
<tr><td>Slave_heartbeat_period</td><td>How often to request a heartbeat packet from the master (in seconds). Returned with <code>SHOW ALL SLAVES STATUS</code> only.</td><td></td></tr>
<tr><td>Gtid_Slave_Pos</td><td>GTID of the last event group replicated on a replica server, for each replication domain, as stored in the <a href="/kb/en/global-transaction-id/#gtid_slave_pos">gtid_slave_pos</a> system variable. Returned with <code>SHOW ALL SLAVES STATUS</code> only.</td><td></td></tr>
<tr><td>SQL_Delay</td><td>Value specified by <code>MASTER_DELAY</code> in <code><a href="/kb/en/change-master-to/">CHANGE MASTER</a></code> (or 0 if none).</td><td><a href="/kb/en/mariadb-1023-release-notes/">MariaDB 10.2.3</a></td></tr>
<tr><td>SQL_Remaining_Delay</td><td>When the replica is delaying the execution of an event due to <code>MASTER_DELAY</code>, this is the number of seconds of delay remaining before the event will be applied. Otherwise, the value is <code>NULL</code>.</td><td><a href="/kb/en/mariadb-1023-release-notes/">MariaDB 10.2.3</a></td></tr>
<tr><td>Slave_SQL_Running_State</td><td>The state of the SQL driver threads, same as in <code><a href="/kb/en/show-processlist/">SHOW PROCESSLIST</a></code>. When the replica is delaying the execution of an event due to <code>MASTER_DELAY</code>, this field displays: "<code>Waiting until MASTER_DELAY seconds after master executed event</code>".</td><td><a href="/kb/en/mariadb-1023-release-notes/">MariaDB 10.2.3</a></td></tr>
<tr><td>Slave_DDL_Groups</td><td>This status variable counts the occurrence of DDL statements.  This is a replica-side counter for optimistic parallel replication.</td><td><a href="/kb/en/mariadb-1037-release-notes/">MariaDB 10.3.7</a></td></tr>
<tr><td>Slave_Non_Transactional_Groups</td><td>This status variable counts the occurrence of non-transactional event groups.  This is a replica-side counter for optimistic parallel replication.</td><td><a href="/kb/en/mariadb-1037-release-notes/">MariaDB 10.3.7</a></td></tr>
<tr><td>Slave_Transactional_Groups</td><td>This status variable counts the occurrence of transactional event groups.  This is a replica-side counter for optimistic parallel replication.</td><td><a href="/kb/en/mariadb-1037-release-notes/">MariaDB 10.3.7</a></td></tr>
</tbody></table>

### SHOW REPLICA STATUS

##### MariaDB starting with [10.5.1](/kb/en/mariadb-1051-release-notes/)

`SHOW REPLICA STATUS` is an alias for `SHOW SLAVE STATUS` from [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/).

## Examples

If you issue this statement using the [mysql](/clients-utilities/mysql-client/) client,
you can use a `<code>\G`</code> statement terminator rather than a semicolon to
obtain a more readable vertical layout.

```sql
SHOW SLAVE STATUS\G
*************************** 1. row ***************************
               Slave_IO_State: Waiting for master to send event
                  Master_Host: db01.example.com
                  Master_User: replicant
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: mariadb-bin.000010
          Read_Master_Log_Pos: 548
               Relay_Log_File: relay-bin.000004
                Relay_Log_Pos: 837
        Relay_Master_Log_File: mariadb-bin.000010
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
              Replicate_Do_DB: 
          Replicate_Ignore_DB: 
           Replicate_Do_Table: 
       Replicate_Ignore_Table: 
      Replicate_Wild_Do_Table: 
  Replicate_Wild_Ignore_Table: 
                   Last_Errno: 0
                   Last_Error: 
                 Skip_Counter: 0
          Exec_Master_Log_Pos: 548
              Relay_Log_Space: 1497
              Until_Condition: None
               Until_Log_File: 
                Until_Log_Pos: 0
           Master_SSL_Allowed: No
           Master_SSL_CA_File: 
           Master_SSL_CA_Path: 
              Master_SSL_Cert: 
            Master_SSL_Cipher: 
               Master_SSL_Key: 
        Seconds_Behind_Master: 0
Master_SSL_Verify_Server_Cert: No
                Last_IO_Errno: 0
                Last_IO_Error: 
               Last_SQL_Errno: 0
               Last_SQL_Error: 
  Replicate_Ignore_Server_Ids: 
             Master_Server_Id: 101
               Master_SSL_Crl: 
           Master_SSL_Crlpath: 
                   Using_Gtid: No
                  Gtid_IO_Pos: 
```

```sql
SHOW ALL SLAVES STATUS\G
*************************** 1. row ***************************
              Connection_name: 
              Slave_SQL_State: Slave has read all relay log; waiting for the slave I/O thread to update it
               Slave_IO_State: Waiting for master to send event
                  Master_Host: db01.example.com
                  Master_User: replicant
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: mariadb-bin.000010
          Read_Master_Log_Pos: 3608
               Relay_Log_File: relay-bin.000004
                Relay_Log_Pos: 3897
        Relay_Master_Log_File: mariadb-bin.000010
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
              Replicate_Do_DB: 
          Replicate_Ignore_DB: 
           Replicate_Do_Table: 
       Replicate_Ignore_Table: 
      Replicate_Wild_Do_Table: 
  Replicate_Wild_Ignore_Table: 
                   Last_Errno: 0
                   Last_Error: 
                 Skip_Counter: 0
          Exec_Master_Log_Pos: 3608
              Relay_Log_Space: 4557
              Until_Condition: None
               Until_Log_File: 
                Until_Log_Pos: 0
           Master_SSL_Allowed: No
           Master_SSL_CA_File: 
           Master_SSL_CA_Path: 
              Master_SSL_Cert: 
            Master_SSL_Cipher: 
               Master_SSL_Key: 
        Seconds_Behind_Master: 0
Master_SSL_Verify_Server_Cert: No
                Last_IO_Errno: 0
                Last_IO_Error: 
               Last_SQL_Errno: 0
               Last_SQL_Error: 
  Replicate_Ignore_Server_Ids: 
             Master_Server_Id: 101
               Master_SSL_Crl: 
           Master_SSL_Crlpath: 
                   Using_Gtid: No
                  Gtid_IO_Pos:
         Retried_transactions: 0
           Max_relay_log_size: 104857600
         Executed_log_entries: 40
    Slave_received_heartbeats: 11
       Slave_heartbeat_period: 1800.000
               Gtid_Slave_Pos: 0-101-2320
```

You can also access some of the variables directly from status variables:

```sql
SET @@default_master_connection="test" ;
show status like "%slave%"

Variable_name   Value
Com_show_slave_hosts    0
Com_show_slave_status   0
Com_start_all_slaves    0
Com_start_slave 0
Com_stop_all_slaves     0
Com_stop_slave  0
Rpl_semi_sync_slave_status      OFF
Slave_connections       0
Slave_heartbeat_period  1800.000
Slave_open_temp_tables  0
Slave_received_heartbeats       0
Slave_retried_transactions      0
Slave_running   OFF
Slaves_connected        0
Slaves_running  1
```

## See Also

- [MariaDB replication](/kb/en/high-availability-performance-tuning-mariadb-replication/)