# Installing and Configuring a Single Server ColumnStore System

After the MariaDB ColumnStore server have been setup based on the Preparing for Installations Document and the required MariaDB ColumnStore Packages have been Installed, use of the following option to configure and install MariaDB ColumnStore:

- MariaDB ColumnStore One Step Quick Installer Script - Release 1.1.6 and later
- MariaDB ColumnStore Post Install Script

This Article shows how to install on a single server system.

If installing a single combined functionality server with no plans to add additional servers, Single Node should be chosen.

Going from one configuration to another will require a re-installation of the MariaDB ColumnStore software.

The following is a transcript of a typical runs of the MariaDB ColumnStore configuration/install scripts. Plain-text formatting indicates output from the script and bold text indicates responses to questions. After each question there is a short discussion of what the question is asking and what some typical answers might be. You will not see these discussions the running the actual configuration script.

NOTE: You can install MariaDB ColumnStore as a root user or non-root user. This is based on how you setup the servers based on "Preparing for Installation Document". If installing as root, you need to be logged in as root user in a root login shell. If you are installing as non-root, you need to be logged in as a non-root user that was setup in the ["Preparing for Installation Document"](/kb/en/preparing-for-columnstore-installation/)

# MariaDB ColumnStore One Step Quick Installer Script, quick_installer_single_server.sh

MariaDB ColumnStore One Step Quick Installer Script, quick_installer_single_server.sh, is used to perform a simple 1 command run to perform the configuration and startup of MariaDB ColumnStore package on a Single Server setup. This will work with both root and non-root installs. Available in Release 1.1.6 and later.

It will perform an install with these defaults:

- System-Name = columnstore-1
- Single-Server Install with 1 Performance Module
- Storage - Internal
- DBRoot - DBroot #1 assigned to Performance Module #1

## Running quick_installer_single_server.sh help as root user

```sql
/usr/local/mariadb/columnstore/bin/quick_installer_single_server.sh --help
Usage ./quick_installer_multi_server.sh

Quick Installer for a Single Server MariaDB ColumnStore Install
```

## Running quick_installer_single_server.sh as root user

```sql
# /usr/local/mariadb/columnstore/bin/quick_installer_single_server.sh
Run post-install script

The next step is:

If installing on a pm1 node:

/usr/local/mariadb/columnstore/bin/postConfigure

If installing on a non-pm1 using the non-distributed option:

/usr/local/mariadb/columnstore/bin/columnstore start


Run postConfigure script


This is the MariaDB ColumnStore System Configuration and Installation tool.
It will Configure the MariaDB ColumnStore System and will perform a Package
Installation of all of the Servers within the System that is being configured.

IMPORTANT: This tool requires to run on the Performance Module #1

With the no-Prompting Option being specified, you will be required to have the following:

 1. Root user ssh keys setup between all nodes in the system or
    use the password command line option.
 2. A Configure File to use to retrieve configure data, default to Columnstore.xml.rpmsave
    or use the '-c' option to point to a configuration file.


===== Quick Install Single-Server Configuration =====

Single-Server install is used when there will only be 1 server configured
on the system. It can also be used for production systems, if the plan is
to stay single-server.
Enter System Name (columnstore-1) >


Performing Configuration Setup and MariaDB ColumnStore Startup 

NOTE: Setting 'NumBlocksPct' to 50%
      Setting 'TotalUmMemory' to 25% of total memory. 

Running the MariaDB ColumnStore setup scripts

post-mysqld-install Successfully Completed
post-mysql-install Successfully Completed

Starting MariaDB Columnstore Database Platform

MariaDB ColumnStore Database Platform Starting, please wait ...... DONE

System Catalog Successfull Created

MariaDB ColumnStore Install Successfully Completed, System is Active

Enter the following command to define MariaDB ColumnStore Alias Commands

. /etc/profile.d/columnstoreAlias.sh

Enter 'mcsmysql' to access the MariaDB ColumnStore SQL console
Enter 'mcsadmin' to access the MariaDB ColumnStore Admin console

NOTE: The MariaDB ColumnStore Alias Commands are in /etc/profile.d/columnstoreAlias.sh
```

## Running quick_installer_single_server.sh as non-root user

```sql
# /home/guest/mariadb/columnstore/bin/quick_installer_single_server.sh
Run post-install script

The next step is:

If installing on a pm1 node:

/home/guest/mariadb/columnstore/bin/postConfigure

If installing on a non-pm1 using the non-distributed option:

/home/guest/mariadb/columnstore/bin/columnstore start


Run postConfigure script


This is the MariaDB ColumnStore System Configuration and Installation tool.
It will Configure the MariaDB ColumnStore System and will perform a Package
Installation of all of the Servers within the System that is being configured.

IMPORTANT: This tool requires to run on the Performance Module #1

With the no-Prompting Option being specified, you will be required to have the following:

 1. Root user ssh keys setup between all nodes in the system or
    use the password command line option.
 2. A Configure File to use to retrieve configure data, default to Columnstore.xml.rpmsave
    or use the '-c' option to point to a configuration file.


===== Quick Install Single-Server Configuration =====

Single-Server install is used when there will only be 1 server configured
on the system. It can also be used for production systems, if the plan is
to stay single-server.
Enter System Name (columnstore-1) >


Performing Configuration Setup and MariaDB ColumnStore Startup 

NOTE: Setting 'NumBlocksPct' to 50%
      Setting 'TotalUmMemory' to 25% of total memory. 

Running the MariaDB ColumnStore setup scripts

post-mysqld-install Successfully Completed
post-mysql-install Successfully Completed

Starting MariaDB Columnstore Database Platform

MariaDB ColumnStore Database Platform Starting, please wait ...... DONE

System Catalog Successfull Created

MariaDB ColumnStore Install Successfully Completed, System is Active

Enter the following command to define MariaDB ColumnStore Alias Commands

. /etc/profile.d/columnstoreAlias.sh

Enter 'mcsmysql' to access the MariaDB ColumnStore SQL console
Enter 'mcsadmin' to access the MariaDB ColumnStore Admin console

NOTE: The MariaDB ColumnStore Alias Commands are in /etc/profile.d/columnstoreAlias.sh
```

# MariaDB ColumnStore Post Install Script, postConfigure

Run the script 'postConfigure' to complete the configuration and installation.

## Common Installation Examples

During postConfigure, this first question will be asked. Select 1 for a Single Server Install

```sql
Select the type of server install [1=single, 2=multi] (1) > 1
```

## Running postConfigure as root user:

```sql
/usr/local/mariadb/columnstore/bin/postConfigure
```

## Running postConfigure as non-root user:

```sql
export COLUMNSTORE_INSTALL_DIR=/home/guest/mariadb/columnstore
export LD_LIBRARY_PATH=/home/guest/mariadb/columnstore/lib:/home/guest/mariadb/columnstore/mysql/lib
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

## Setup System Server Type Configuration

```sql
There are 2 options when configuring the System Server Type: single and multi

  'single'  - Single-Server install is used when there will only be 1 server configured
              on the system. It can also be used for production systems, if the plan is
              to stay single-server.

  'multi'   - Multi-Server install is used when you want to configure multiple servers now or
              in the future. With Multi-Server install, you can still configure just 1 server
              now and add on addition servers/modules in the future.

Select the type of System Server install [1=single, 2=multi] (2) >  1

Performing a Single Server Install.
Enter System Name (columnstore-1) > mymcs1
```

<strong>Notes</strong>: You should give this system a name that will appear in various Admin utilities, SNMP messages,
etc. The name can be composed of any number of printable characters and spaces.

## Setup High Availability Data Storage Mount Configuration

```sql
There are 2 options when configuring the storage: internal and external

  'internal' -    This is specified when a local disk is used for the DBRoot storage.
                  High Availability Server Failover is not Supported in this mode

  'external' -    This is specified when the DBRoot directories are mounted.
                  High Availability Server Failover is Supported in this mode.
<<code>>
Select the type of Data Storage [1=internal, 2=external] (1) > **<Enter>**
```

Notes: Choosing internal and using softlinks to point to an externally mounted storage will allow you to
use any format (i.e., ext2, ext3, etc.).

```sql
Enter the list (Nx,Ny,Nz) or range (Nx-Nz) of dbroot IDs assigned to module 'pm1'
(1) > 1
```

<strong>Notes</strong>: The installer will set up the number of dbroot directories based on this answer.

## Performing Configuration Setup and MariaDB Columnstore Startup

```sql
NOTE: Setting 'NumBlocksPct' to 50%
            Setting 'TotalUmMemory' to 25% of total memory (Combined Server Install maximum value is 16G). 
             Value set to 4G
```

<strong>Notes</strong>: The default maximum for a single server is 16Gb.

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

## MariaDB Columnstore Memory Configuration

During the installation process, postConfigure will set the 2 main Memory configuration settings based on the size of memory detected on the local node.

The 2 settings are in the MariaDB Columnstore Configuration file,  /usr/local/mariadb/columnstore/etc Columnstore.xml. These 2 settings are:

```sql
'NumBlocksPct'   - Performance Module Data cache memory setting

TotalUmMemory  - User Module memory setting, used as temporary memory for joins
```

On a Single Server Install, this is the default settings:

```sql
NumBlocksPct    - 50% of total memory

TotalUmMemory - 25% of total memory, default maximum the percentage equal to 16G
```

The user can choose to change these settings after the install is completed, if for instance they want to setup more memory for Joins to utilize. On a single server or combined UM/PM server, it is recommended to not
have the combination of these 2 settings over 75% of total memory.