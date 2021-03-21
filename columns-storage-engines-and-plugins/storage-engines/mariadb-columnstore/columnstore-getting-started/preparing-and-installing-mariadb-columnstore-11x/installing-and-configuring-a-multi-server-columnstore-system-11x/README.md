# Installing and Configuring a Multi Server ColumnStore System - 1.1.X

## Preparing to Install

After the MariaDB ColumnStore servers have been setup based on the [Preparing for Installations](/kb/en/preparing-for-columnstore-installation/) document and the required MariaDB ColumnStore Packages have been Installed, use of the following option to configure and install MariaDB ColumnStore:

- MariaDB ColumnStore One Step Quick Installer Script - Release 1.1.6 and later
- MariaDB ColumnStore Post Install Script

<strong>NOTE</strong>: The install and setup had you install the packages on the node designed as Performance Module #1, 'pm1'. This is where the install script is run from, 'pm1'

If installing on a system where there is a need to multi servers initially planned in the future, you would be a multi server install instead of a single server install.

Going from one configuration to another will require a re-installation of the MariaDB ColumnStore software.

<strong>NOTE</strong>: You can install MariaDB ColumnStore as a root user or non-root user. This is based on how you setup the servers based on "Preparing for Installation Document". If installing as root, you need to be logged in as root user in a root login shell. If you are installing as non-root, you need to be logged in as a non-root user that was setup in the "Preparing for Installation Document".

[https://mariadb.com/kb/en/library/preparing-for-columnstore-installation-11x/](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-getting-started/preparing-and-installing-mariadb-columnstore-11x/preparing-for-columnstore-installation-11x)

## ColumnStore Cluster Test Tool

This tool can be running before doing installation on a single-server or multi-node installs. It will verify the setup of all servers that are going to be used in the Columnstore System.

[https://mariadb.com/kb/en/mariadb/mariadb-columnstore-cluster-test-tool](https://mariadb.com/kb/en/mariadb/mariadb-columnstore-cluster-test-tool)

## MariaDB ColumnStore One Step Quick Installer Script, quick_installer_multi_server.sh

MariaDB ColumnStore One Step Quick Installer Script, quick_installer_multi_server.sh, is used to perform a simple 1 command run to perform the configuration and startup of MariaDB ColumnStore package on a Mutil Server setup. This will work with both root and non-root installs. Available in Release 1.1.6 and later.

The script has 4 parameters.

- --pm-ip-addresses=xxx.xxx.xxx.xxx,xxx.xxx.xxx.xxx
- --um-ip-addresses=xxx.xxx.xxx.xxx,xxx.xxx.xxx.xxx, optional
- --dist-install Use Distributed Install, optional
- --system-name=nnnn System Name, optional

It will perform an install with these defaults:

- System-Name = columnstore-1, when not specified
- Multi-Server Install
<ul start="1"><li>with X Number of PMs when only PM IP Addresses are provided
</li><li>with X Number of UMs when UM IP Addresses are provided and X Number of UMs when PM IP Addresses are provided
</li></ul>
- Non-Distributed Installer when --dist-install when not specified
- Storage - Internal
- DBRoot - 1 DBroot per 1 Performance Module
- Local Query is disabled on um/pm install
- MariaDB Replication is enabled
- ssk-keys setup required

### Running quick_installer_multi_server.sh help as root user

```sql
# /usr/local/mariadb/columnstore/bin/quick_installer_multi_server.sh --help
Usage ./quick_installer_multi_server.sh [OPTION]

Quick Installer for a Multi Server MariaDB ColumnStore Install

Defaults to non-distrubuted install, meaning MariaDB Columnstore
needs to be preinstalled on all nodes in the system

Performace Module (pm) IP addresses are required
User Module (um) IP addresses are option
When only pm IP addresses provided, system is combined setup
When both pm/um IP addresses provided, system is seperate setup

--pm-ip-addresses=xxx.xxx.xxx.xxx,xxx.xxx.xxx.xxx
--um-ip-addresses=xxx.xxx.xxx.xxx,xxx.xxx.xxx.xxx, optional
--dist-install Use Distributed Install, optional
--system-name=nnnn System Name, optional
```

### Running quick_installer_multi_server.sh for a 1um/1pm non-distributed system as root user

```sql
# /usr/local/mariadb/columnstore/bin/quick_installer_multi_server.sh --um-ip-addresses=10.128.0.4 --pm-ip-addresses=10.128.0.3

NOTE: Performing a Multi-Server Seperate install with um and pm running on seperate servers

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


===== Quick Install Multi-Server Configuration =====



Setup System Module Type Configuration

There are 2 options when configuring the System Module Type: separate and combined

  'separate' - User and Performance functionality on separate servers.

  'combined' - User and Performance functionality on the same server

Select the type of System Module Install [1=separate, 2=combined] (1) > 

Seperate Server Installation will be performed.

NOTE: Local Query Feature allows the ability to query data from a single Performance
      Module. Check MariaDB ColumnStore Admin Guide for additional information.

Enable Local Query feature? [y,n] (n) > 

NOTE: The MariaDB ColumnStore Schema Sync feature will replicate all of the
      schemas and InnoDB tables across the User Module nodes. This feature can be enabled
      or disabled, for example, if you wish to configure your own replication post installation.

MariaDB ColumnStore Schema Sync feature is Enabled, do you want to leave enabled? [y,n] (y) > 


NOTE: MariaDB ColumnStore Replication Feature is enabled

NOTE: MariaDB ColumnStore Non-Distributed Install Feature is enabled

Enter System Name (columnstore-1) > 


Setup Storage Configuration


----- Setup Performance Module DBRoot Data Storage Mount Configuration -----

There are 2 options when configuring the storage: internal or external

  'internal' -    This is specified when a local disk is used for the DBRoot storage.
                  High Availability Server Failover is not Supported in this mode

  'external' -    This is specified when the DBRoot directories are mounted.
                  High Availability Server Failover is Supported in this mode.

Select the type of Data Storage [1=internal, 2=external] (1) > 

===== Setup Memory Configuration =====

NOTE: Setting 'NumBlocksPct' to 70%
      Setting 'TotalUmMemory' to 50%

Setup the Module Configuration


----- User Module Configuration -----

Enter number of User Modules [1,1024] (1) > 

*** User Module #1 Configuration ***

Enter Nic Interface #1 Host Name (10.128.0.4) > 
Enter Nic Interface #1 IP Address of 10.128.0.4 (10.128.0.4) > 

----- Performance Module Configuration -----

Enter number of Performance Modules [1,1024] (1) > 

*** Parent OAM Module Performance Module #1 Configuration ***

Enter Nic Interface #1 Host Name (10.128.0.3) > 
Enter Nic Interface #1 IP Address of 10.128.0.3 (10.128.0.3) > 
Enter the list (Nx,Ny,Nz) or range (Nx-Nz) of DBRoot IDs assigned to module 'pm1' (1) > 

Next step is to enter the password to access the other Servers.
This is either your password or you can default to using a ssh key
If using a password, the password needs to be the same on all Servers.

Enter password, hit 'enter' to default to using a ssh key, or 'exit' > 

===== Checking MariaDB ColumnStore System Logging Functionality =====

The MariaDB ColumnStore system logging is setup and working on local server

MariaDB ColumnStore System Configuration and Installation is Completed

===== MariaDB ColumnStore System Startup =====

System Configuration is complete.
Performing System Installation.

----- Starting MariaDB ColumnStore on local server -----

MariaDB ColumnStore successfully started

MariaDB ColumnStore Database Platform Starting, please wait ........ DONE

System Catalog Successfully Created

Run MariaDB ColumnStore Replication Setup..  DONE

MariaDB ColumnStore Install Successfully Completed, System is Active

Enter the following command to define MariaDB ColumnStore Alias Commands

. /etc/profile.d/columnstoreAlias.sh

Enter 'mcsmysql' to access the MariaDB ColumnStore SQL console
Enter 'mcsadmin' to access the MariaDB ColumnStore Admin console

NOTE: The MariaDB ColumnStore Alias Commands are in /etc/profile.d/columnstoreAlias.sh
```

### Running quick_installer_multi_server.sh for a 2 pm combo distributed install system as non-root user

```sql
# /home/guest/mariadb/columnstore/bin/quick_installer_multi_server.sh --pm-ip-addresses=10.128.0.3,10.128.0.4 --dist-install

NOTE: Performing a Multi-Server Combined install with um/pm running on some server

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


===== Quick Install Multi-Server Configuration =====



Setup System Module Type Configuration

There are 2 options when configuring the System Module Type: separate and combined

  'separate' - User and Performance functionality on separate servers.

  'combined' - User and Performance functionality on the same server

Select the type of System Module Install [1=separate, 2=combined] (2) > 

Combined Server Installation will be performed.
The Server will be configured as a Performance Module.
All MariaDB ColumnStore Processes will run on the Performance Modules.

NOTE: The MariaDB ColumnStore Schema Sync feature will replicate all of the
      schemas and InnoDB tables across the User Module nodes. This feature can be enabled
      or disabled, for example, if you wish to configure your own replication post installation.

MariaDB ColumnStore Schema Sync feature is Enabled, do you want to leave enabled? [y,n] (y) > 


NOTE: MariaDB ColumnStore Replication Feature is enabled

Enter System Name (columnstore-1) > 


Setup Storage Configuration


----- Setup Performance Module DBRoot Data Storage Mount Configuration -----

There are 2 options when configuring the storage: internal or external

  'internal' -    This is specified when a local disk is used for the DBRoot storage.
                  High Availability Server Failover is not Supported in this mode

  'external' -    This is specified when the DBRoot directories are mounted.
                  High Availability Server Failover is Supported in this mode.

Select the type of Data Storage [1=internal, 2=external] (1) > 

===== Setup Memory Configuration =====


NOTE: Setting 'NumBlocksPct' to 50%
      Setting 'TotalUmMemory' to 25%

Setup the Module Configuration


----- Performance Module Configuration -----

Enter number of Performance Modules [1,1024] (2) > 

*** Parent OAM Module Performance Module #1 Configuration ***

Enter Nic Interface #1 Host Name (10.128.0.3) > 
Enter Nic Interface #1 IP Address of 10.128.0.3 (10.128.0.3) > 
Enter the list (Nx,Ny,Nz) or range (Nx-Nz) of DBRoot IDs assigned to module 'pm1' (1) > 

*** Performance Module #2 Configuration ***

Enter Nic Interface #1 Host Name (10.128.0.4) > 
Enter Nic Interface #1 IP Address of 10.128.0.4 (10.128.0.4) > 
Enter the list (Nx,Ny,Nz) or range (Nx-Nz) of DBRoot IDs assigned to module 'pm2' (2) > 

===== Running the MariaDB ColumnStore MariaDB Server setup scripts =====


post-mysqld-install Successfully Completed
post-mysql-install Successfully Completed

Next step is to enter the password to access the other Servers.
This is either your password or you can default to using a ssh key
If using a password, the password needs to be the same on all Servers.

Enter password, hit 'enter' to default to using a ssh key, or 'exit' > 

===== System Installation =====

System Configuration is complete.
Performing System Installation.

Performing a MariaDB ColumnStore System install using Binary packages
located in the /home/guest directory.



----- Performing Install on 'pm2 / 10.128.0.4' -----

Install log file is located here: /tmp/pm2_binary_install.log


MariaDB ColumnStore Package being installed, please wait ...  DONE

===== Checking MariaDB ColumnStore System Logging Functionality =====

The MariaDB ColumnStore system logging is setup and working on local server

===== MariaDB ColumnStore System Startup =====

System Configuration is complete.
Performing System Installation.

----- Starting MariaDB ColumnStore on local server -----

MariaDB ColumnStore successfully started

MariaDB ColumnStore Database Platform Starting, please wait .............. DONE

System Catalog Successfully Created

Run MariaDB ColumnStore Replication Setup..  DONE

MariaDB ColumnStore Install Successfully Completed, System is Active

Enter the following command to define MariaDB ColumnStore Alias Commands

. /etc/profile.d/columnstoreAlias.sh

Enter 'mcsmysql' to access the MariaDB ColumnStore SQL console
Enter 'mcsadmin' to access the MariaDB ColumnStore Admin console

NOTE: The MariaDB ColumnStore Alias Commands are in /etc/profile.d/columnstoreAlias.sh

```

## MariaDB ColumnStore Post Install Script, postConfigure

The following is a transcript of a typical run of the MariaDB ColumnStore configuration script. Plain-text formatting indicates output from the script and bold text indicates responses to questions. After each question there is a short discussion of what the question is asking and what some typical answers might be. You will not see these discussions the running the actual configuration script.

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

## Post Configuration tool

Post Configuration tool, postConfigure, does the system configuration and setup. The servers and storages are configured during this process. It is executed from the designed 'pm1' node.

### postConfigure script

To get additional information, user to run the following:

```sql
# /usr/local/mariadb/columnstore/bin/postConfigure -h

This is the MariaDB ColumnStore System Configuration and Installation tool.
It will Configure the MariaDB ColumnStore System based on Operator inputs and
will perform a Package Installation of all of the Modules within the
System that is being configured.

IMPORTANT: This tool should only be run on a Performance Module Server,
           preferably Module #1

Instructions:

	Press 'enter' to accept a value in (), if available or
	Enter one of the options within [], if available, or
	Enter a new value


Usage: postConfigure [-h][-c][-u][-p][-s][-port][-i][-n]
   -h  Help
   -c  Config File to use to extract configuration data, default is Columnstore.xml.rpmsave
   -u  Upgrade, Install using the Config File from -c, default to Columnstore.xml.rpmsave
	    If ssh-keys aren't setup, you should provide passwords as command line arguments
   -p  Unix Password, used with no-prompting option
   -s  Single Threaded Remote Install
   -port MariaDB ColumnStore Port Address
   -i Non-root Install directory, Only use for non-root installs
   -n Non-distributed install, meaning it will not install the remote nodes
```

### postConfigure Install options

The postConfigure script supports 2 different types of installs:

- Distributed Install
- Non-Distributed Install

#### Distributed Install

Distributed Install by postConfigure will interact with all the nodes in the system during the configuration and setup process. It will push copied of the MariaDB ColumnStore packages from the 'pm1' node where postConfigure is running. During upgrades, it will also make sure the ColumnStore is stopped and will replace any packages that were previously install with the new package.
Since it pushed the new packages to the other nodes, it is required that the packages be placed in the current home directory of the user doing the install, i.e. /root/ for root user install.

Distributed Install is the default setting, so no additional command line arguments need to be provide.

#### Non-Distributed Install

Non-Distributed Install by postConfigure will not have any interaction excluding a ping test during the configuration section to validate the IP address provided. With this option, it is up to the user to install the MariaDB ColumnStore packages on the non-pm1 nodes and startup the ColumnStore service before running postConfigure.

Non-Distributed Install does require an additional command line argument to be provided.
The "-n" is the required argument as sown in this example:

```sql
/usr/local/mariadb/columnstore/bin/postConfigure -n
```

### Running postConfigure

Running postConfigure is a bit different when launching as root user or a non-root user. As a non-root user, you will be required the setup 2 Environment variables and provide the base directory where MariaDB ColumnStore resides.

#### Running postConfigure as root user

```sql
/usr/local/mariadb/columnstore/bin/postConfigure
```

#### Running postConfigure as non-root user

```sql
export COLUMNSTORE_INSTALL_DIR=/home/guest/mariadb/columnstore
export LD_LIBRARY_PATH=/home/guest/mariadb/columnstore/lib:/home/guest/mariadb/columnstore/mysql/lib/mysql
/home/guest/mariadb/columnstore/bin/postConfigure -i /home/guest/mariadb/columnstore
```

### postConfigure examples

#### Distributed Install example

This is an example of a Distributed Install of a 1UM/2PM system with internal storage.
MariaDB ColumnStore 'rpm's packages was installed for this example. No password is provide on the command line, so the root user password is provided when prompted.

```sql
# /usr/local/mariadb/columnstore/bin/postConfigure

This is the MariaDB ColumnStore System Configuration and Installation tool.
It will Configure the MariaDB ColumnStore System and will perform a Package
Installation of all of the Servers within the System that is being configured.

IMPORTANT: This tool should only be run on the Parent OAM Module
           which is a Performance Module, preferred Module #1

Prompting instructions:

	Press 'enter' to accept a value in (), if available or
	Enter one of the options within [], if available, or
	Enter a new value


===== Setup System Server Type Configuration =====

There are 2 options when configuring the System Server Type: single and multi

  'single'  - Single-Server install is used when there will only be 1 server configured
              on the system. It can also be used for production systems, if the plan is
              to stay single-server.

  'multi'   - Multi-Server install is used when you want to configure multiple servers now or
              in the future. With Multi-Server install, you can still configure just 1 server
              now and add on addition servers/modules in the future.

Select the type of System Server install [1=single, 2=multi] (2) > 


===== Setup System Module Type Configuration =====

There are 2 options when configuring the System Module Type: separate and combined

  'separate' - User and Performance functionality on separate servers.

  'combined' - User and Performance functionality on the same server

Select the type of System Module Install [1=separate, 2=combined] (2) > 1

Separate Server Installation will be performed.

NOTE: Local Query Feature allows the ability to query data from a single Performance
      Module. Check MariaDB ColumnStore Admin Guide for additional information.

Enable Local Query feature? [y,n] (n) > 

NOTE: The MariaDB ColumnStore Schema Sync feature will replicate all of the
      schemas and InnoDB tables across the User Module nodes. This feature can be enabled
      or disabled, for example, if you wish to configure your own replication post installation.

MariaDB ColumnStore Schema Sync feature, do you want to enable? [y,n] (y) > 

NOTE: MariaDB ColumnStore Replication Feature is enabled

Enter System Name (columnstore-1) > mymcs-1


===== Setup Storage Configuration =====


----- Setup Performance Module DBRoot Data Storage Mount Configuration -----

There are 3 options when configuring the storage: internal or external

  'internal' -    This is specified when a local disk is used for the DBRoot storage.
                  High Availability Server Failover is not Supported in this mode

  'external' -    This is specified when the DBRoot directories are mounted.
                  High Availability Server Failover is Supported in this mode.

Select the type of Data Storage [1=internal, 2=external] (1) > 

===== Setup Memory Configuration =====

NOTE: Setting 'NumBlocksPct' to 70%
      Setting 'TotalUmMemory' to 50%

===== Setup the Module Configuration =====


----- User Module Configuration -----

Enter number of User Modules [1,1024] (1) > 

*** User Module #1 Configuration ***

Enter Nic Interface #1 Host Name (unassigned) > um1-hostname
Enter Nic Interface #1 IP Address of um1-hostname (0.0.0.0) > 172.30.0.59
Enter Nic Interface #2 Host Name (unassigned) > 

----- Performance Module Configuration -----

Enter number of Performance Modules [1,1024] (1) > 2

*** Parent OAM Module Performance Module #1 Configuration ***

Enter Nic Interface #1 Host Name (ip-172-30-0-161.us-west-2.compute.internal) > 
Enter Nic Interface #1 IP Address of ip-172-30-0-161.us-west-2.compute.internal (172.30.0.161) > 
Enter Nic Interface #2 Host Name (unassigned) > 
Enter the list (Nx,Ny,Nz) or range (Nx-Nz) of DBRoot IDs assigned to module 'pm1' (1) > 

*** Performance Module #2 Configuration ***

Enter Nic Interface #1 Host Name (unassigned) > pm2-hostname
Enter Nic Interface #1 IP Address of pm2-hostname (0.0.0.0) > 172.30.0.152
Enter Nic Interface #2 Host Name (unassigned) > 
Enter the list (Nx,Ny,Nz) or range (Nx-Nz) of DBRoot IDs assigned to module 'pm2' () > 2

===== System Installation =====

System Configuration is complete.
Performing System Installation.

Performing a MariaDB ColumnStore System install using RPM packages
located in the /root directory.

Next step is to enter the password to access the other Servers.
This is either your password or you can default to using a ssh key
If using a password, the password needs to be the same on all Servers.

Enter password, hit 'enter' to default to using a ssh key, or 'exit' > 
Confirm password > 

----- Performing Install on 'um1 / um1-hostname' -----

Install log file is located here: /tmp/um1_rpm_install.log


----- Performing Install on 'pm2 / pm2-hostname' -----

Install log file is located here: /tmp/pm2_rpm_install.log


MariaDB ColumnStore Package being installed, please wait ...  DONE

===== Checking MariaDB ColumnStore System Logging Functionality =====

The MariaDB ColumnStore system logging is setup and working on local server

===== MariaDB ColumnStore System Startup =====

System Installation is complete. If any part of the install failed,
the problem should be investigated and resolved before continuing.

package installed and the associated service started.

Would you like to startup the MariaDB ColumnStore System? [y,n] (y) > 

----- Starting MariaDB ColumnStore on local server -----

MariaDB ColumnStore successfully started

MariaDB ColumnStore Database Platform Starting, please wait ......... DONE

System Catalog Successfully Created

MariaDB ColumnStore Install Successfully Completed, System is Active

Enter the following command to define MariaDB ColumnStore Alias Commands

. /usr/local/mariadb/columnstore/bin/columnstoreAlias

Enter 'mcsmysql' to access the MariaDB ColumnStore SQL console
Enter 'mcsadmin' to access the MariaDB ColumnStore Admin console

# 
```

IMPORTANT: If postConfigure fails at any point, you can use the following guides to help trouble shoot any issues. And once an issue has been fixed, it is required that you re-run postConfigure until you get a successful completion.

[https://mariadb.com/kb/en/library/system-troubleshooting-mariadb-columnstore/#multi-node-install-problems-and-how-to-diagnose](https://mariadb.com/kb/en/library/system-troubleshooting-mariadb-columnstore/#multi-node-install-problems-and-how-to-diagnose)

#### Non-Distributed Install example

This is an example of a Non-Distributed Install of a 2PM system with External storage. MariaDB ColumnStore 'rpm's packages was installed for this example. A password of 'ssh' is provide on the command line, which means that ssh-keys are setup and no password prompt will be required.

This example also was done on a system that had the GlusterFS third party package installed, so it will show the storage option for the MariaDB ColumnStore Data Replication. If GlusterFS wasnt installed, it would not show up as a Storage Option.

```sql
# /usr/local/mariadb/columnstore/bin/postConfigure -n -p ssh

This is the MariaDB ColumnStore System Configuration and Installation tool.
It will Configure the MariaDB ColumnStore System and will perform a Package
Installation of all of the Servers within the System that is being configured.

IMPORTANT: This tool should only be run on the Parent OAM Module
           which is a Performance Module, preferred Module #1

Prompting instructions:

	Press 'enter' to accept a value in (), if available or
	Enter one of the options within [], if available, or
	Enter a new value


===== Setup System Server Type Configuration =====

There are 2 options when configuring the System Server Type: single and multi

  'single'  - Single-Server install is used when there will only be 1 server configured
              on the system. It can also be used for production systems, if the plan is
              to stay single-server.

  'multi'   - Multi-Server install is used when you want to configure multiple servers now or
              in the future. With Multi-Server install, you can still configure just 1 server
              now and add on addition servers/modules in the future.

Select the type of System Server install [1=single, 2=multi] (2) > 


===== Setup System Module Type Configuration =====

There are 2 options when configuring the System Module Type: separate and combined

  'separate' - User and Performance functionality on separate servers.

  'combined' - User and Performance functionality on the same server

Select the type of System Module Install [1=separate, 2=combined] (2) > 

Separate Server Installation will be performed.

NOTE: The MariaDB ColumnStore Schema Sync feature will replicate all of the
      schemas and InnoDB tables across the User Module nodes. This feature can be enabled
      or disabled, for example, if you wish to configure your own replication post installation.

MariaDB ColumnStore Schema Sync feature, do you want to enable? [y,n] (y) > 

NOTE: MariaDB ColumnStore Replication Feature is enabled

Enter System Name (columnstore-1) > mymcs-1


===== Setup Storage Configuration =====


----- Setup Performance Module DBRoot Data Storage Mount Configuration -----

There are 3 options when configuring the storage: internal or external

  'internal' -    This is specified when a local disk is used for the DBRoot storage.
                  High Availability Server Failover is not Supported in this mode

  'external' -    This is specified when the DBRoot directories are mounted.
                  High Availability Server Failover is Supported in this mode.

Select the type of Data Storage [1=internal, 2=external] (1) > 2

===== Setup Memory Configuration =====

NOTE: Setting 'NumBlocksPct' to 70%
      Setting 'TotalUmMemory' to 50%

===== Setup the Module Configuration =====

----- Performance Module Configuration -----

Enter number of Performance Modules [1,1024] (1) > 2

*** Parent OAM Module Performance Module #1 Configuration ***

Enter Nic Interface #1 Host Name (ip-172-30-0-161.us-west-2.compute.internal) > 
Enter Nic Interface #1 IP Address of ip-172-30-0-161.us-west-2.compute.internal (172.30.0.161) > 
Enter Nic Interface #2 Host Name (unassigned) > 
Enter the list (Nx,Ny,Nz) or range (Nx-Nz) of DBRoot IDs assigned to module 'pm1' (1) > 

*** Performance Module #2 Configuration ***

Enter Nic Interface #1 Host Name (unassigned) > pm2-hostname
Enter Nic Interface #1 IP Address of pm2-hostname (0.0.0.0) > 172.30.0.152
Enter Nic Interface #2 Host Name (unassigned) > 
Enter the list (Nx,Ny,Nz) or range (Nx-Nz) of DBRoot IDs assigned to module 'pm2' () > 2

===== Running the MariaDB ColumnStore MariaDB ColumnStore setup scripts =====

post-mysqld-install Successfully Completed
post-mysql-install Successfully Completed

===== Checking MariaDB ColumnStore System Logging Functionality =====

The MariaDB ColumnStore system logging is setup and working on local server

MariaDB ColumnStore System Configuration and Installation is Completed

===== MariaDB ColumnStore System Startup =====

System Installation is complete. If any part of the install failed,
the problem should be investigated and resolved before continuing.

Non-Distributed Install: make sure all other modules have MariaDB ColumnStore
package installed and the associated service started.

----- Starting MariaDB ColumnStore on local server -----

MariaDB ColumnStore successfully started

MariaDB ColumnStore Database Platform Starting, please wait ......... DONE

System Catalog Successfully Created

MariaDB ColumnStore Install Successfully Completed, System is Active

Enter the following command to define MariaDB ColumnStore Alias Commands

. /usr/local/mariadb/columnstore/bin/columnstoreAlias

Enter 'mcsmysql' to access the MariaDB ColumnStore SQL console
Enter 'mcsadmin' to access the MariaDB ColumnStore Admin console

# 
```

IMPORTANT: If postConfigure fails at any point, you can use the following guides to help trouble shoot any issues. And once an issue has been fixed, it is required that you re-run postConfigure until you get a successful completion.

[https://mariadb.com/kb/en/library/system-troubleshooting-mariadb-columnstore/#multi-node-install-problems-and-how-to-diagnose](https://mariadb.com/kb/en/library/system-troubleshooting-mariadb-columnstore/#multi-node-install-problems-and-how-to-diagnose)

#### Data Replication Install example

This is an example of a Distributed Install of a 1UM/2PM system with Data Replication storage. Root user password is passed in as a command line argument.

As part of the Preparing for Install, the GlusterFS third party package would have already been installed on all of the PM servers within this system. During the running of postConfigure, the user will be required to enter some Data Replication options based on the type of system you want. Example is number of copies of the Database you would like to have maintained across the PM servers.

This example also enabled the Local Query Feature.

```sql
/usr/local/mariadb/columnstore/bin/postConfigure -p 'root-user-password'

This is the MariaDB ColumnStore System Configuration and Installation tool.
It will Configure the MariaDB ColumnStore System and will perform a Package
Installation of all of the Servers within the System that is being configured.

IMPORTANT: This tool should only be run on the Parent OAM Module
           which is a Performance Module, preferred Module #1

Prompting instructions:

	Press 'enter' to accept a value in (), if available or
	Enter one of the options within [], if available, or
	Enter a new value


===== Setup System Server Type Configuration =====

There are 2 options when configuring the System Server Type: single and multi

  'single'  - Single-Server install is used when there will only be 1 server configured
              on the system. It can also be used for production systems, if the plan is
              to stay single-server.

  'multi'   - Multi-Server install is used when you want to configure multiple servers now or
              in the future. With Multi-Server install, you can still configure just 1 server
              now and add on addition servers/modules in the future.

Select the type of System Server install [1=single, 2=multi] (2) > 


===== Setup System Module Type Configuration =====

There are 2 options when configuring the System Module Type: separate and combined

  'separate' - User and Performance functionality on separate servers.

  'combined' - User and Performance functionality on the same server

Select the type of System Module Install [1=separate, 2=combined] (2) > 1

Seperate Server Installation will be performed.

NOTE: Local Query Feature allows the ability to query data from a single Performance
      Module. Check MariaDB ColumnStore Admin Guide for additional information.

Enable Local Query feature? [y,n] (n) > y

NOTE: Local Query Feature is enabled

NOTE: MariaDB ColumnStore Replication Feature is enabled

Enter System Name (columnstore-1) > mymcs-1


===== Setup Storage Configuration =====


----- Setup Performance Module DBRoot Data Storage Mount Configuration -----

There are 3 options when configuring the storage: internal, external, or DataRedundancy

  'internal' -    This is specified when a local disk is used for the DBRoot storage.
                  High Availability Server Failover is not Supported in this mode

  'external' -    This is specified when the DBRoot directories are mounted.
                  High Availability Server Failover is Supported in this mode.

  'DataRedundancy' - This is specified when gluster is installed and you want
                  the DBRoot directories to be controlled by ColumnStore Data Redundancy.
                  High Availability Server Failover is Supported in this mode.
                  NOTE: glusterd service must be running and enabled on all PMs.

Select the type of Data Storage [1=internal, 2=external, 3=DataRedundancy] (1) > 3

===== Setup Memory Configuration =====

NOTE: Setting 'NumBlocksPct' to 70%
      Setting 'TotalUmMemory' to 50%

===== Setup the Module Configuration =====


----- User Module Configuration -----

Enter number of User Modules [1,1024] (1) > 

*** User Module #1 Configuration ***

Enter Nic Interface #1 Host Name (unassigned) > um1-hostname
Enter Nic Interface #1 IP Address of um1-hostname (0.0.0.0) > 172.30.0.59
Enter Nic Interface #2 Host Name (unassigned) > 

----- Performance Module Configuration -----

Enter number of Performance Modules [1,1024] (1) > 2

*** Parent OAM Module Performance Module #1 Configuration ***

Enter Nic Interface #1 Host Name (ip-172-30-0-161.us-west-2.compute.internal) > 
Enter Nic Interface #1 IP Address of ip-172-30-0-161.us-west-2.compute.internal (172.30.0.161) > 
Enter Nic Interface #2 Host Name (unassigned) > 
Enter the list (Nx,Ny,Nz) or range (Nx-Nz) of DBRoot IDs assigned to module 'pm1' (1) > 

*** Performance Module #2 Configuration ***

Enter Nic Interface #1 Host Name (unassigned) > pm2-hostname
Enter Nic Interface #1 IP Address of pm2-hostname (0.0.0.0) > 172.30.0.152
Enter Nic Interface #2 Host Name (unassigned) > 
Enter the list (Nx,Ny,Nz) or range (Nx-Nz) of DBRoot IDs assigned to module 'pm2' () > 2

===== System Installation =====

System Configuration is complete.
Performing System Installation.


Performing a MariaDB ColumnStore System install using RPM packages
located in the /root directory.


===== Running the MariaDB ColumnStore MariaDB ColumnStore setup scripts =====

post-mysqld-install Successfully Completed
post-mysql-install Successfully Completed

----- Performing Install on 'um1 / um1-hostname' -----

Install log file is located here: /tmp/um1_rpm_install.log


----- Performing Install on 'pm2 / pm2-hostname' -----

Install log file is located here: /tmp/pm2_rpm_install.log


MariaDB ColumnStore Package being installed, please wait ...  DONE


===== Configuring MariaDB ColumnStore Data Redundancy Functionality =====


Only 2 PMs configured. Setting number of copies at 2.

----- Setup Data Redundancy Network Configuration -----

  'existing'  -   This is specified when using previously configured network devices. (NIC Interface #1)
                  No additional network configuration is required with this option.

  'dedicated' -   This is specified when it is desired for Data Redundancy traffic to use
                  a separate network than one previously configured for ColumnStore.
                  You will be prompted to provide Hostname and IP information for each PM.


Select the data redundancy network [1=existing, 2=dedicated] (1) > 

----- Performing Data Redundancy Configuration -----

gluster peer probe 172.30.0.161
gluster peer probe 172.30.0.152
Gluster create and start volume dbroot1...DONE
Gluster create and start volume dbroot2...DONE

----- Data Redundancy Configuration Complete -----


===== Checking MariaDB ColumnStore System Logging Functionality =====

The MariaDB ColumnStore system logging is setup and working on local server

===== MariaDB ColumnStore System Startup =====

System Installation is complete. If any part of the install failed,
the problem should be investigated and resolved before continuing.

package installed and the associated service started.

Would you like to startup the MariaDB ColumnStore System? [y,n] (y) > 

----- Starting MariaDB ColumnStore on local server -----

MariaDB ColumnStore successfully started

MariaDB ColumnStore Database Platform Starting, please wait .......... DONE

System Catalog Successfully Created

MariaDB ColumnStore Install Successfully Completed, System is Active

Enter the following command to define MariaDB ColumnStore Alias Commands

. /usr/local/mariadb/columnstore/bin/columnstoreAlias

Enter 'mcsmysql' to access the MariaDB ColumnStore SQL console
Enter 'mcsadmin' to access the MariaDB ColumnStore Admin console

# 
```

IMPORTANT: If postConfigure fails at any point, you can use the following guides to help trouble shoot any issues. And once an issue has been fixed, it is required that you re-run postConfigure until you get a successful completion.

[https://mariadb.com/kb/en/library/system-troubleshooting-mariadb-columnstore/#multi-node-install-problems-and-how-to-diagnose](https://mariadb.com/kb/en/library/system-troubleshooting-mariadb-columnstore/#multi-node-install-problems-and-how-to-diagnose)

## MariaDB Columnstore Memory Configuration

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