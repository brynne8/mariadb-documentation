# ColumnStore Multiple User Module Guide

## Introduction

This Document describes the setup and the functionality of the MariaDB ColumnStore User Module in a Multiple Node configuration. It will detail the different ways to configure how queries are processed with the Multiple Nodes and how the MariaDB ColumnStore Replication works.

## ColumnStore user module

The ColumnStore User Module manages and controls the operation of end-user queries. For additional details on this can be found here:

[https://mariadb.com/kb/en/mariadb/columnstore-user-module/](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-architecture/columnstore-user-module/)

## ColumnStore user module configuration

A MariaDB ColumnStore system will have at least 1 User Module. It might reside on the same server as the MariaDB ColumnStore Performance Module or can reside on a separate server. A MariaDB ColumnStore system can also be configured to have more than 1 User Modules. 
The advantages of having Multiple User Modules:

- Higher concurrency queries execution by distributing the queries across all User Modules
- Higher Query Performance
- Provides User Modules High Availability into the system
- Provides support of MariaDB ColumnStore User Module Replication

A MariaDB ColumnStore system can be configured with Multiple User Modules either during the install installation phase when running the configuration script postConfigure. 
More details can be found here:

[https://mariadb.com/kb/en/mariadb/installing-and-configuring-a-multi-server-columnstore-system/](https://mariadb.com/kb/en/mariadb/installing-and-configuring-a-multi-server-columnstore-system/)

An existing MariaDB ColumnStore system can be scaled out by adding addition User Modules. More Details can be found here on adding addition modules to a system:

## ColumnStore user module status

Use the 'mcsadmin getSystemInfo' to display the Module Status along with the current Master User Module, which is called "Primary Front-End MariaDB ColumnStore Module"

Here is an example:

```sql
# mcsadmin  getSystemInfo
getsysteminfo   Tue Jun 12 13:45:21 2018

System 1.1.4

System and Module statuses

Component     Status                       Last Status Change
------------  --------------------------   ------------------------
System        ACTIVE                       Tue Jun 12 13:41:25 2018

Module um1    ACTIVE                       Tue Jun 12 13:41:14 2018
Module um2    ACTIVE                       Tue Jun 12 13:41:00 2018
Module pm1    ACTIVE                       Tue Jun 12 13:40:49 2018

Active Parent OAM Performance Module is 'pm1'
Primary Front-End MariaDB ColumnStore Module is 'um1'
MariaDB ColumnStore Replication Feature is enabled

MariaDB ColumnStore Process statuses

Process             Module    Status            Last Status Change        Process ID
------------------  ------    ---------------   ------------------------  ----------
ProcessMonitor      um1       ACTIVE            Tue Jun 12 13:40:34 2018        2231
ServerMonitor       um1       ACTIVE            Tue Jun 12 13:40:52 2018        2560
DBRMWorkerNode      um1       ACTIVE            Tue Jun 12 13:40:51 2018        2573
ExeMgr              um1       ACTIVE            Tue Jun 12 13:41:05 2018        3548
DDLProc             um1       ACTIVE            Tue Jun 12 13:41:12 2018        4522
DMLProc             um1       ACTIVE            Tue Jun 12 13:41:24 2018        5098
mysqld              um1       ACTIVE            Tue Jun 12 13:41:15 2018        2519

ProcessMonitor      um2       ACTIVE            Tue Jun 12 13:40:37 2018        2220
ServerMonitor       um2       ACTIVE            Tue Jun 12 13:40:56 2018        2569
DBRMWorkerNode      um2       ACTIVE            Tue Jun 12 13:41:01 2018        2582
ExeMgr              um2       ACTIVE            Tue Jun 12 13:41:03 2018        2779
DDLProc             um2       COLD_STANDBY      Tue Jun 12 13:41:00 2018
DMLProc             um2       COLD_STANDBY      Tue Jun 12 13:41:00 2018
mysqld              um2       ACTIVE            Tue Jun 12 13:40:59 2018        2528

ProcessMonitor      pm1       ACTIVE            Tue Jun 12 13:39:53 2018        1833
ProcessManager      pm1       ACTIVE            Tue Jun 12 13:39:59 2018        1892
DBRMControllerNode  pm1       ACTIVE            Tue Jun 12 13:40:42 2018        2702
ServerMonitor       pm1       ACTIVE            Tue Jun 12 13:40:46 2018        2716
DBRMWorkerNode      pm1       ACTIVE            Tue Jun 12 13:40:45 2018        2731
DecomSvr            pm1       ACTIVE            Tue Jun 12 13:40:47 2018        2828
PrimProc            pm1       ACTIVE            Tue Jun 12 13:40:55 2018        2906
WriteEngineServer   pm1       ACTIVE            Tue Jun 12 13:40:57 2018        2953

Active Alarm Counts: Critical = 0, Major = 0, Minor = 0, Warning = 0, Info = 0

```

[https://mariadb.com/kb/en/mariadb/managing-columnstore-module-configurations/](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/managing-columnstore/managing-columnstore-system/managing-columnstore-module-configurations/)

## ColumnStore multiple user module query execution

Each of the User Modules have a MariaDB server process (mysqld) that that receive a query request from the MariaDB console or from remote applications via the MariaDB Port interface (defaulted is 3306). The MariaDB server process will send that request to the MariaDB ColumnStore process ExeMgr for processing.
More details about how this is processed can be found here:

[https://mariadb.com/kb/en/mariadb/columnstore-user-module/](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-architecture/columnstore-user-module/)

## ColumnStore cross engine joins

MariaDB ColumnStore allows columnstore tables to be joined with non-columnstore tables (e.g. InnoDB tables) within a query. The standard configuration is to process these on the local User Module where the request was made. So the Automatic Query Round-Robin Distribution functionality doesn't apply to this type of queries.

More information on ColumnStore Cross Engine Joins can be found here:

[https://mariadb.com/kb/en/mariadb/configuring-columnstore-cross-engine-joins/](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/managing-columnstore/managing-columnstore-database-environment/configuring-columnstore-cross-engine-joins/)

### Automatic query round-robin distribution

In a standard Multi-Node configuration install with more than 1 User Module, the query request are scaled-out across all User Modules using an automatic round-robin distribution functionality. This means that the MariaDB server process will distribute the query requests to all User Modules (ExeMgrs) in the MariaDB ColumnStore system. The ExeMgr will handle the processing of the query request and pass back the resulting data to the initial MariaDB server process, which will in turn provide that result set to the calling client. This round-robin distribution is handled on a session by sessions basis.

<img src="/kb/en/columnstore-multiple-user-module-guide/+image/um-roundrobin-msg" alt="um-roundrobin-msg" title="um-roundrobin-msg">

### Localized query distribution

It is also possible to override the default round-robin logic and route queries only to the ExeMgr on the same host as the MariaDB server. With large result sets this may be more optimal as it avoids remote data transfers when the ExeMgr is not local.  So what this means that when a Query comes into the MariaDB server process on UM1, it will send it to the ExeMgr on UM1 only for process. And the same would apply to the other User Modules. UM2 MariaDB Server process will send to UM2 ExeMgr. So if the user has a reason to want to keep all or a certain group of queries being process on 1 node, like UM1 and then use UM2 for backup only or maybe to handle special queries, then would be a reason for this type of configuration.

To do this, the ExeMgr(s) port addresses in the MariaDB Columnstore configuration file (Columnstore.xml) would need to be updated after the install to contain the loop back address of 127.0.0.1.

Here is the steps to achieve that. Note this examples shows update a 2 User Module system with 2 ExeMgrs. If you had 3 or more, than you would run the setConfig command the additional times.

Here is the steps, run from PM1: (reminder, all changes to the config file need to be made on PM1)

```sql
# mcsadmin stopSystem y
# /usr/local/mariadb/columnstore/bin/setConfig ExeMgr1 IPAddr 127.0.0.1
# /usr/local/mariadb/columnstore/bin/setConfig ExeMgr2 IPAddr 127.0.0.1
# mcsadmin startsystem
```

NOTE: The steps are assuming a root install directory, change for an non-root install where the directory is different.

NOTE: To go from a Localized query distribution back to a query round-robin distribution, you would either need to run the same commands above, but replacing the 127.0.0.1 with the real IP Addresses of the 2 servers.
Or do a shutdown and run postConfigure again from pm1.

<img src="/kb/en/columnstore-multiple-user-module-guide/+image/um-local-msg" alt="um-local-msg" title="um-local-msg">

## ColumnStore local performance module query

MariaDB ColumnStore support Local Performance Module Query where a query can be run locally on a Performance Module when a system is configured to have the User Module and the Performance Module on different servers. In this case, the Performance Module will have the User Module process running like the MariaDB Server Process and ExeMgr. But they are only there to process local queries. There will not process queries as part of the Automatic Query Round-Robin Distribution.

More information on ColumnStore Local Performance Module Query can be found here:

[https://mariadb.com/kb/en/mariadb/configuring-columnstore-local-pm-query-mode/](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/managing-columnstore/managing-columnstore-database-environment/configuring-columnstore-local-pm-query-mode/)

<img src="/kb/en/columnstore-multiple-user-module-guide/+image/pm-local-query" alt="pm-local-query" title="pm-local-query">

## ColumnStore user module replication

MariaDB Columnstore supports User Module Replication. This will synchronize the the User Module data, which consist of the MariaDB Columnstore Database Schemas and the non-columnstore engine schemas and data. When enabled, it will synchronize the data from the Master Data Replication module, default is User Module 1, to all of the other User Modules including the Local Performance Modules. It uses the standard MySQL Data replication functionality to do this from the MariaDB Server Process.

Which the setup to distribute from the Master User Module, any DDL and non-columnstore table creation and population must be done from the Master User Module. This will keep all of the User Module Databases in-sync.

MariaDB ColumnStore utilizing the same Standard Replication that is used in the MariaDB Server application though the use of binlogs.

The Master User Module will be assigned in the my.cnf file with SERVER-ID of 1. User Module #1 is the default Master at system startup. In the case of a failover where UM1 goes offline and another User Modules takes over as the Master, that User Module will be updated as SERVER-ID of 1. When User Module #1 recovers, it will be assigned a non 1 ID.

### Enabling replication

The ColumnStore User Module Replication functionality can be enabled a couple of different ways:

- During the initial configuration setup in postConfigure
- Using the MariaDB ColumnStore Admin console

```sql
# mcsadmin enableMySQLReplication
```

### Disabling replication

The ColumnStore User Module Replication functionality can be disabled a couple of different ways:

- During the initial configuration setup in postConfigure
- Using the MariaDB ColumnStore Admin console

```sql
# mcsadmin disableMySQLReplication
```

The option to disable the MariaDB ColumnStore User Module Replication is available since there are additional third party tools that can be used to do the Data Replication.

### MariaDB Database password

If you set a root password within the MariaDB Database, you will need to create a .my.cnf file as shown here and it will need to reside on all servers that have a User Module MariaDB running.

[https://mariadb.com/kb/en/library/mariadb-columnstore-system-usage/#mysql-root-user-password](https://mariadb.com/kb/en/library/mariadb-columnstore-system-usage/#mysql-root-user-password)