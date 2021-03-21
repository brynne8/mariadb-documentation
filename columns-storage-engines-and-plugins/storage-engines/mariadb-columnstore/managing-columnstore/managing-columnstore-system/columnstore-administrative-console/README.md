# ColumnStore Administrative Console

The MariaDB ColumnStore Management Console allows you to configure, monitor, and manage the MariaDB ColumnStore system and servers. 
Once you have a running ColumnStore cluster, you can invoke the console from any of the [UM](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-architecture/columnstore-user-module/) or [PM](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-architecture/columnstore-performance-module/) nodes. The console utility is called mcsadmin

```sql
[myuser@srv1~]# mcsadmin
MariaDB Columnstore Admin Console
   enter 'help' for list of commands
   enter 'exit' to exit the MariaDB Columnstore Command Console
   use up/down arrows to recall commands
Active Alarm Counts: Critical = 0, Major = 0, Minor = 0, Warning = 0, Info = 0
Critical Active Alarms:

mcsadmin> quit
```

You can use one of the following commands in the console

```sql
Command                           Description
------------------------------    --------------------------------------------------------
?                                 Get help on the Console Commands
addDbroot                         Add DBRoot Disk storage to the MariaDB Columnstore System
addModule                         Add a Module within the MariaDB Columnstore System
alterSystem-disableModule         Disable a Module and Alter the MariaDB Columnstore System
alterSystem-enableModule          Enable a Module and Alter the MariaDB Columnstore System
assignDbrootPmConfig              Assign unassigned DBroots to Performance Module
assignElasticIPAddress            Assign Amazon Elastic IP Address to a module
disableLog                        Disable the levels of process and debug logging
disableMySQLReplication           Disable MySQL Replication functionality on the system
enableLog                         Enable the levels of process and debug logging
enableMySQLReplication            Enable MySQL Replication functionality on the system
exit                              Exit from the Console tool
findObjectFile                    Get the name of the directory containing the first file of the object
getActiveAlarms                   Get Active Alarm list
getActiveSQLStatements            Get List Active SQL Statements within the System
getAlarmConfig                    Get Alarm Configuration Information
getAlarmHistory                   Get system alarms
getAlarmSummary                   Get Summary counts of Active Alarm
getLogConfig                      Get the System log file configuration
getModuleConfig                   Get Module Name Configuration Information
getModuleCpu                      Get a Module CPU usage
getModuleCpuUsers                 Get a Module Top Processes utilizing CPU
getModuleDisk                     Get a Module Disk usage
getModuleHostNames                Get a list of Module host names (NIC 1 only)
getModuleMemory                   Get a Module Memory usage
getModuleMemoryUsers              Get a Module Top Processes utilizing Memory
getModuleResourceUsage            Get a Module Resource usage
getModuleTypeConfig               Get Module Type Configuration Information
getProcessConfig                  Get Process Configuration Information
getProcessStatus                  Get MariaDB Columnstore Process Statuses
getSoftwareInfo                   Get the MariaDB Columnstore Package information
getStorageConfig                  Get System Storage Configuration Information
getStorageStatus                  Get System Storage Status
getSystemCpu                      Get System CPU usage on all modules
getSystemCpuUsers                 Get System Top Processes utilizing CPU
getSystemDirectories              Get System Installation and Temporary Logging Directories
getSystemDisk                     Get System Disk usage on all modules
getSystemInfo                     Get the Over-all System Statuses
getSystemMemory                   Get System Memory usage on all modules
getSystemMemoryUsers              Get System Top Processes utilizing Memory
getSystemNetworkConfig            Get System Network Configuration Information
getSystemResourceUsage            Get System Resource usage on all modules
getSystemStatus                   Get System and Modules Status
help                              Get help on the Console Commands
monitorAlarms                     Monitor alarms in realtime mode
movePmDbrootConfig                Move DBroots from one Performance Module to another
quit                              Exit from the Console tool
redistributeData                  Redistribute table data accross all dbroots to balance disk usage
removeDbroot                      Remove DBRoot Disk storage from the MariaDB Columnstore System
removeModule                      Remove a Module within the MariaDB Columnstore System
resetAlarm                        Resets an Active Alarm
restartSystem                     Restarts a stopped or shutdown MariaDB Columnstore System
resumeDatabaseWrites              Resume performing writes to the MariaDB Columnstore Database
setAlarmConfig                    Set a Alarm Configuration parameter
setModuleTypeConfig               Set a Module Type Configuration parameter
setProcessConfig                  Set a Process Configuration parameter
shutdownSystem                    Shuts down the MariaDB Columnstore System
startSystem                       Starts a stopped or shutdown MariaDB Columnstore System
stopSystem                        Stops the processing of the MariaDB Columnstore System
suspendDatabaseWrites             Suspend performing writes to the MariaDB Columnstore Database
switchParentOAMModule             Switches the Active Parent OAM Module to another Performance Module
system                            Execute a system shell command
unassignDbrootPmConfig            Unassign DBroots from a Performance Module
unassignElasticIPAddress          Unassign Amazon Elastic IP Address
```

### Help command

The help command displays supported commands. You can view brief help definitions or verbose definitions. You can also enter partial command names with the help command to view verbose definitions.

For example, type help <em>enableLog</em> to get the verbose definition of the <em>enableLog</em> command as shown  below.

```sql
mcsadmin> 
mcsadmin> help enableLog
help   Fri Jun 10 19:26:26 2016

   Command:     enableLog

   Description: Enable the levels of process and debug logging

   Arguments:   Required: 'system' or Module-name where logging is being enabled
                Required: 'all' or the specific level to enable
                    Levels: critical, error, warning, info, and debug

mcsadmin> 
```

### Case sensitivity

Commands are not case sensitive; however parameters and device names, like server and processes, are
case sensitive.

### Recall command history

Use the up and down arrow keys on your keyboard to scroll through the command history.

### Command repeat option

Commands can be run continuously using the “-r” option. 
The repeat option repeats a command every 5 seconds. You can change the repeat interval to be between 1 and 60 seconds by adding the number of seconds after the command.

This is useful to check status in real-time mode. For example to repeat the <em>GetProcessStatus</em> command every 2 seconds:

```sql
mcsadmin> getProcessStatus -r2
repeating the command 'GetProcessStatus' every 2 seconds, enter CTRL-D to stop

getprocessstatus   Fri Jun 10 19:31:28 2016

MariaDB Columnstore Process statuses

Process             Module    Status            Last Status Change        Process ID
------------------  ------    ---------------   ------------------------  ----------
ProcessMonitor      pm1       ACTIVE            Fri Jun 10 01:50:04 2016        2487
ProcessManager      pm1       ACTIVE            Fri Jun 10 01:50:10 2016        2673
SNMPTrapDaemon      pm1       ACTIVE            Fri Jun 10 01:50:16 2016        3534
DBRMControllerNode  pm1       ACTIVE            Fri Jun 10 01:50:20 2016        3585
ServerMonitor       pm1       ACTIVE            Fri Jun 10 01:50:22 2016        3625
DBRMWorkerNode      pm1       ACTIVE            Fri Jun 10 01:50:22 2016        3665
DecomSvr            pm1       ACTIVE            Fri Jun 10 01:50:26 2016        3742
PrimProc            pm1       ACTIVE            Fri Jun 10 01:50:28 2016        3770
ExeMgr              pm1       ACTIVE            Fri Jun 10 01:50:32 2016        3844
WriteEngineServer   pm1       ACTIVE            Fri Jun 10 01:50:36 2016        3934
DDLProc             pm1       ACTIVE            Fri Jun 10 01:50:40 2016        3991
DMLProc             pm1       ACTIVE            Fri Jun 10 01:50:45 2016        4058
mysqld              pm1       ACTIVE            Fri Jun 10 01:50:22 2016        2975

```

### Unix Command Line entry

mcsadmin commands can be run from the linux command line prompt

```sql
# mcsadmin getSystemInfo
```

### Command Name Short-cutting

mcsadmin commands can be short-cutted. This example would execute getSystemInfo. The is the first command it would find alphabetically with 'getsystemi'

```sql
# mcsadmin getsystemi
```