# ColumnStore System Monitoring Configuration

# Introduction

ColumnStore is designed to be somewhat self managing and healing. The following 2 processes help achieve this:

- <strong>ProcMon</strong> runs on each node and is responsible for ensuring that the other required ColumnStore processes are started and automatically restarted as appropriate on that server. This in turn is started and monitored by the run.sh shell script which ensures it is restarted should it be killed. The run.sh script is invoked and automatically started by the <strong>columnstore</strong> systemd service at bootup time. This can also be utilized to restart the service on an individual node though generally it is preferred to use the <em>mcsadmin stop, shutdown, and start</em> commands from the PM1 node.
- <strong>ProcMgr</strong> runs on each PM node with only one taking an active role at a time, the others remaining in warm standby mode. This process manager is responsible for overall system health, resource monitoring, and PM node failover management.

To provide additional monitoring guarantees, an external monitoring tool should monitor the health of these 3 processes and potentially all. If the run.sh process fails then the system is at potential risk of not being able to self heal.

# System monitoring configuration

A number of system configuration variables exist to allow fine tuning of the system monitoring capabilities. In general the default values will work relatively well for many cases.

The configuration parameters are maintained in the /usr/local/mariadb/columnstore/etc/Columnstore.xml file. In a multiple server deployment these should only be edited on the PM1 server as this will be automatically replicated to other servers by the system.  A system restart will be required for the configuration change to take affect.

Convenience utility programs <em>getConfig</em> and <em>setConfig</em> are available to safely update the Columnstore.xml without needing to be comfortable with editing XML files. The -h argument will display usage information. The section value will be <em>SystemConfig</em> for all settings in this document. For example:

```sql
# ./setConfig SystemConfig ModuleHeartbeatPeriod 5
# ./getConfig SystemConfig ModuleHeartbeatPeriod
5
```

## Module heartbeats

Heartbeat monitoring occurs between modules (both [UM](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-architecture/columnstore-user-module) and [PM](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-architecture/columnstore-performance-module)) to determine the module is up and functioning. The module heartbeat settings are the same for all modules.

1 <em>ModuleHeartbeatPeriod</em> refers to how often the heartbeat test is performed. For example, if you set the period to 5, then the heartbeat test is performed every 5 seconds. The initial default value is 1. To disable heartbeat monitoring set the value to -1.
2 <em>ModuleHeartbeatCount</em>  refers to how many failures in a row must take place before a fault is processed. The initial default value is 3.

## Disk threshold

Thresholds can be set to trigger a local alert when file system usage crosses a specified percentage of a file system on a server. Critical, Major or Minor thresholds can be set for the disk usage for each server. However it is recommend to use an external system monitoring tool configured to monitor for free disk space to perform proactive external alerting or paging. Actual columnstore data is stored within the <em>data&lt;N&gt;</em> directories of the installation and mariadb db files are stored under the <em>mysql/db</em> directory.

1 <em>ExternalMinorThreshold</em> - Percentage threshold for when a minor local alarm is triggered. Default value is 70.
2 <em>ExternalMajorThreshold</em> - Percentage threshold for when a minor local alarm is triggered. Default value is 80.
3 <em>ExternalCriticalThreshold</em> - Percentage threshold for when a minor local alarm is triggered. Default value is 90.

The value is a numeric percentage value between 0 and 100. To disable a particular threshold use value 0.
To disable a threshold alarm, set it to 0.

## Memory utilization

A couple of mcsadmin commands provide convenience functions for monitoring memory utilization across nodes. <em>getSystemMemory</em> returns server level memory statistics and <em>getSystemMemoryUsers</em> shows the the top 5 processes by server. The following examples are for a 2 server combined setup:

```sql
mcsadmin> getSystemMemory
getsystemmemory   Tue Nov 29 11:14:21 2016

System Memory Usage per Module (in K bytes)

Module  Mem Total  Mem Used  Cache    Mem Usage %  Swap Total  Swap Used  Swap Usage % 
------  ---------  --------  -------  -----------  ----------  ---------  ------------ 
pm1     7979488    1014772   6438432      12       3145724     0               0
pm2     3850724    632712    1134324      16       3145724     0               0

mcsadmin> getSystemMemoryUsers
getsystemmemoryusers   Tue Nov 29 11:41:10 2016

System Process Top Memory Users per Module

Module 'pm1' Top Memory Users (in bytes)

Process             Memory Used  Memory Usage %
-----------------   -----------  --------------
mysqld              19621              3
PrimProc            18990              3
gnome-shell         10192              2
systemd-journald    4236               1
DDLProc             3004               1

Module 'pm2' Top Memory Users (in bytes)

Process             Memory Used  Memory Usage %
-----------------   -----------  --------------
mysqld              19046              5
PrimProc            18891              5
ProcMon             2343               1
workernode          1806               1
WriteEngineServ     1507               1


```

# Viewing storage configuration

To view the storage configuration, use the <em>getStorageConfig</em> command in [mcsadmin](/kb/en/mariadb-columnstore-administrative-console/), or simply use [mcsadmin](/kb/en/mariadb-columnstore-administrative-console/) <em>getStorageConfig</em> from the operating system prompt. This will provide information on DBRoots and which PM they are assigned to, if any.

Example:

```sql
# mcsadmin getstorageconfig Wed Mar 28 10:40:34 2016

System Storage Configuration

Storage Type = internal
System DBRoot count = 6
DBRoot IDs assigned to 'pm1' = 1
DBRoot IDs assigned to 'pm2' = 2
DBRoot IDs assigned to 'pm3' = 3
DBRoot IDs assigned to 'pm4' = 4
DBRoot IDs assigned to 'pm5' = 5
DBRoot IDs assigned to 'pm6' = 6
```

# Module monitoring configuration

An internal alarm system is used to keep track of internal notable events as a convenience or reference point. It is recommended to use a dedicated system monitoring tool for more proactive alerting of critical CPU, memory, or disk utilization issues for each of the servers.

Alarms are logged to the <em> /var/log/mariadb/columnstore/alarm.log</em> file and a summary is displayed in mcsadmin. The <em>getActiveAlarms</em> command in mcsadmin can be used to retrieve current alarm conditions.

For each module (PM and UM), the following resource monitoring parameters can be configured:

<table><tbody><tr><th>Resource Monitoring Parameter</th><th>mcsadmin command</th></tr>
<tr><td>CPU thresholds</td><td>setModuleTypeConfig (module name) ModuleCPU(Clear/ Minor/Major/Critical)Threshold n (where n= percentage of CPU usage)</td></tr>
<tr><td>Disk file system use threshold</td><td>setModuleTypeConfig (module name) ModuleDisk(Minor/ Major/Critical)Threshold n (where n= percentage of disk system used)</td></tr>
<tr><td>Module swap thresholds</td><td>setModuleTypeConfig (module name) ModuleSwap(Minor/ Major/Crictical)Threshold n (where n= percentage of swap space used)</td></tr>
</tbody></table>

## Alarm trigger count threshold

For an alarm, a threshold can be set for how many times the alarm can be triggered in 30 minutes. The default threshold is 100.

```sql
setAlarmConfig (alarmID#) Threshold n
```

(where n= maximum number of times an alarm can be triggered in 30 minutes),

Example to change Alarm ID 22's threshold to 50:

```sql
# mcsadmin setAlarmConfig 22 Threshold 50
```

## Clearing alarms

The <em>resetAlarm</em> command is used to clear and acknowledge the issue is resolved. The <em>resetAlarm</em> command can be invoked with the argument ALL to clear all outstanding local alarms.

## Automated restart based on excessive swapping

ColumnStore by default has behavior that will restart a server should swap space utilization exceed the configured module swap major threshold (default is 80%). At this point the system will likely be near unusable and so this is an attempt to recover from very large queries or data loads.  The behavior of this is configured by the <em>SystemConfig</em> section configuration variable <em>SwapAction</em> which contains the oam command to be run if the threshold is exceeded. The default value is <em>'restartSystem'</em> but it can be set to <em>'none'</em> to disable this behavior. The fact that this has happened can be determined by the following log entry:

```sql
Nov 01 11:23:13 [ServerMonitor] 13.306324 |0|0|0| C 09 CAL0000: Swap Space usage over Major threashold, perform OAM command restartSystem
```

# Logging level management

There are five levels of logging in MariaDB ColumnStore.

- Critical
- Error
- Warning
- Info
- Debug

Application log files are written to <em> /var/log/mariadb/columnstore</em> on each server and log rotation / archiving is configured to manage these automatically.

To get details about current logging configuration:

```sql
# mcsadmin getlogconfig
getlogconfig   Wed Oct 19 06:58:47 2016

MariaDB Columnstore System Log Configuration Data

System Logging Configuration File being used: /etc/rsyslog.d/49-columnstore.conf

Module    Configured Log Levels
------    ---------------------------------------
pm1       Critical Error Warning Info
```

The system logging configuration file referenced is a standard syslog configuration file and may be edited to enable and or disable specific levels, for example to disable debug logging and to only log at the specific level in each file:

```sql
# cat /etc/rsyslog.d/49-columnstore.conf
# MariaDb Columnstore Database Platform Logging
local1.=crit -/var/log/mariadb/columnstore/crit.log
local1.=err -/var/log/mariadb/columnstore/err.log
local1.=warning -/var/log/mariadb/columnstore/warning.log
local1.=info -/var/log/mariadb/columnstore/info.log
```

After making changes to this restart the syslog process, e.g:

```sql
# systemctl restart rsyslog
```

Log rotation and archiving are also configured by the installer and the settings for this may be found and managed similarly in the file <em> /etc/logrotate.d/columnstore </em>. If the current log files are manually deleted restart the syslog process to resume logging.