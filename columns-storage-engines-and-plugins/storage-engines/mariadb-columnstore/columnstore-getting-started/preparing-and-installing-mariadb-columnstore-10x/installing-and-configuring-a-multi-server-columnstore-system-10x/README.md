# Installing and Configuring a Multi Server ColumnStore System - 1.0.X

After the MariaDB ColumnStore servers have been setup based on the [Preparing for Installations](/kb/en/preparing-for-columnstore-installation/) document and the required MariaDB ColumnStore Packages have been Installed, use of the following option to configure and install MariaDB ColumnStore:

- MariaDB ColumnStore Post Install Script

<strong>NOTE</strong>: The install and setup had you install the packages on the node designed as Performance Module #1, 'pm1'. This is where the install script is run from, 'pm1'

If installing on a system where there is a need to multi servers initially planned in the future, you would be a multi server install instead of a single server install.

Going from one configuration to another will require a re-installation of the MariaDB ColumnStore software.

<strong>NOTE</strong>: You can install MariaDB ColumnStore as a root user or non-root user. This is based on how you setup the servers based on "Preparing for Installation Document". If installing as root, you need to be logged in as root user in a root login shell. If you are installing as non-root, you need to be logged in as a non-root user that was setup in the "Preparing for Installation Document".

## ColumnStore Cluster Test Tool

This tool can be running before doing installation on a single-server or multi-node installs. It will verify the setup of all servers that are going to be used in the Columnstore System.

[https://mariadb.com/kb/en/mariadb/mariadb-columnstore-cluster-test-tool](https://mariadb.com/kb/en/mariadb/mariadb-columnstore-cluster-test-tool)

The following is a transcript of a typical run of the MariaDB ColumnStore configuration script. Plain-text formatting indicates output from the script and bold text indicates responses to questions. After each question there is a short discussion of what the question is asking and what some typical answers might be. You will not see these discussions the running the actual configuration script.

## MariaDB ColumnStore Post Install Script, postConfigure

Run the script 'postConfigure' to complete the configuration and installation. This Article shows how to install on a mullti server system.

### Common Installation Examples

During postConfigure, there are 2 questions that are asked where the answer given determines the path
that postConfigure takes in configuring the system. Those 2 questions are as follows:

```sql
Select the type of server install [1=single, 2=multi] (2) >
```

and

```sql
Select the Type of Module Install being performed:
1. Separate - User and Performance functionalities on separate servers
2. Combined - User and Performance functionalities on the same server
Enter Server Type ID [1-2] (1) >
```

The following examples illustrates some common configurations and helps to provide answers to the above
questions:

- Single Node - User and Performance running on 1 server - single / combined
- Mutli-Node #1 -  User and Performance running on some server - multi / combined
- Mutli-Node #2 -  User and Performance running on separate servers - multi / separate

### Running postConfigure as root user:

```sql
/usr/local/mariadb/columnstore/bin/postConfigure
```

### Running postConfigure as non-root user:

```sql
export COLUMNSTORE_INSTALL_DIR=/home/guest/mariadb/columnstore
export LD_LIBRARY_PATH=/home/guest/mariadb/columnstore/lib:/home/guest/mariadb/columnstore/mysql/lib/mysql
/home/guest/mariadb/columnstore/bin/postConfigure -i /home/guest/mariadb/columnstore
```

This is the MariaDB Columnstore System Configuration and Installation tool.
It will Configure the MariaDB Columnstore System and will perform a Package
Installation of all of the Servers within the System that is being configured.

IMPORTANT: This tool should only be run on the Parent OAM Module
           which is a Performance Module, preferred Module #1

Prompting instructions:

Press 'enter' to accept a value in (), if available or
	Enter one of the options within [], if available, or
	Enter a new value

### Setup System Server Type Configuration

There are 2 options when configuring the System Server Type: single and multi

'single'  - Single-Server install is used when there will only be 1 server configured
              on the system. It can also be used for production systems, if the plan is
              to stay single-server.

'multi'   - Multi-Server install is used when you want to configure multiple servers now or
              in the future. With Multi-Server install, you can still configure just 1 server
              now and add on addition servers/modules in the future.

Select the type of System Server install [1=single, 2=multi] (2) &gt; 2

### Setup System Module Type Configuration

There are 2 options when configuring the System Module Type: separate and combined

'separate' - User and Performance functionality on separate servers.

'combined' - User and Performance functionality on the same server

Select the type of System Module Install [1=separate, 2=combined] (2) &gt; 1

Seperate Server Installation will be performed.

NOTE: Local Query Feature allows the ability to query data from a single Performance
      Module. See [Local PM Query Mode](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/managing-columnstore/managing-columnstore-database-environment/configuring-columnstore-local-pm-query-mode/) for additional information.

```sql
Enable Local Query feature? [y,n] (n) > n

NOTE: The MariaDB ColumnStore Schema Sync feature will replicate all of the
      schemas and InnoDB tables across the User Module nodes. This feature can be enabled
      or disabled, for example, if you wish to configure your own replication post installation.

MariaDB ColumnStore Schema Sync feature, do you want to enable? [y,n] (y) > y

Enter System Name (columnstore-1) > mymcs1
```

Notes: You should give this system a name that will appear in various Admin utilities,
etc. The name can be composed of any number of printable characters and spaces.

### Setup High Availability Data Storage Mount Configuration

There are 2 options when configuring the storage: internal and external

'internal' -    This is specified when a local disk is used for the DBRoot storage.
                  High Availability Server Failover is not Supported in this mode

'external' -    This is specified when the DBRoot directories are mounted.
                  High Availability Server Failover is Supported in this mode.

```sql
Select the type of Data Storage [1=internal, 2=external] (1) > <Enter>
```

Notes: Choosing internal and using softlinks to point to an externally mounted storage will allow you to
use any format (i.e., ext2, ext3, etc.).

### Setup the Module Configuration

```sql
----- User Module Configuration -----

Enter number of User Modules [1,1024] (1) > 1

*** User Module #1 Configuration ***

Enter Nic Interface #1 Host Name (unassigned) > server-um1
Enter Nic Interface #1 IP Address of server-um1 (172.30.0.171) >
Enter Nic Interface #2 Host Name (unassigned) >

----- Performance Module Configuration -----

Enter number of Performance Modules [1,1024] (1) > 1

*** Parent OAM Module Performance Module #1 Configuration ***

Enter Nic Interface #1 Host Name (unassigned) > server-pm1
Enter Nic Interface #1 IP Address of server-pm1 (172.30.0.171) >
Enter Nic Interface #2 Host Name (unassigned) >


Enter the list (Nx,Ny,Nz) or range (Nx-Nz) of DBRoot IDs assigned to module 'pm1' (1) > 1

*** Performance Module #2 Configuration ***

Enter Nic Interface #1 Host Name (unassigned) > server-pm2
Enter Nic Interface #1 IP Address of server-pm1 (172.30.0.171) >
Enter Nic Interface #2 Host Name (unassigned) >

Enter the list (Nx,Ny,Nz) or range (Nx-Nz) of DBRoot IDs assigned to module 'pm2' (2) > 2

```

### Performing Configuration Setup and MariaDB Columnstore Startup

NOTE: Setting 'NumBlocksPct' to 50%
            Setting 'TotalUmMemory' to 25% of total memory (Combined Server Install maximum value is 16G). 
             Value set to 4G

Notes: The default maximum for a single server is 16Gb.

```sql
Running the MariaDB Columnstore MySQL setup scripts

post-mysqld-install Successfully Completed
post-mysql-install Successfully Completed

Starting MariaDB ColumnStore Database Platform

Starting MariaDB ColumnStore Database Platform, please wait......... DONE

System Catalog Successfully Created

MariaDB ColumnStore Install Successfully Completed, System is Active

Enter the following command to define MariaDB ColumnStore Alias Commands

. /usr/local/mariadb/columnstore/bin/columnstoreAlias

Enter 'mcsmysql' to access the MariaDB Columnstore MySQL console
Enter 'mcsadmin' to access the MariaDB Columnstore Admin console

```

#### MariaDB Columnstore Memory Configuration

During the installation process, postConfigure will set the 2 main Memory configuration settings based on the size of memory detected on the local node.

The 2 settings are in the MariaDB Columnstore Configuration file,  /usr/local/mariadb/columnstore/etc Columnstore.xml. These 2 settings are:

```sql
'NumBlocksPct'   - Performance Module Data cache memory setting

TotalUmMemory  - User Module memory setting, used as temporary memory for joins
```

On a system that has the Performance Module and User Module functionality combined on the same server, this is the default settings:

```sql
NumBlocksPct    - 50% of total memory

TotalUmMemory - 25% of total memory, default maximum the percentage equal to 16G
```

On a system that has the Performance Module and User Module functionality on different servers, this is the default settings:

```sql
NumBlocksPct    - This setting is NOT configured, and the default that the applications will then use is 70%

TotalUmMemory - 50% of total memory 
```

The user can choose to change these settings after the install is completed, if for instance they want to setup more memory for Joins to utilize. On a single server or combined UM/PM server, it is recommended to not
have the combination of these 2 settings over 75% of total memory