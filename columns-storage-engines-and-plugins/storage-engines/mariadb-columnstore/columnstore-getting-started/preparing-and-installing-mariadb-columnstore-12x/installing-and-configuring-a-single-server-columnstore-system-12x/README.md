# Installing and Configuring a Single Server ColumnStore System - 1.2.x

## Preparing to Install

Review the [Preparing for Installations Document](/kb/en/preparing-for-columnstore-installation-12x/)  and ensure that any necessary pre-requisites have been completed on the target servers including installing the ColumnStore software packages.

If installing a single combined functionality server with no plans to add additional servers, Single Node should be chosen. Going from single server to multi-server configuration to another will require a re-installation of the MariaDB ColumnStore software.

## MariaDB ColumnStore Quick Installer for a single server system

The script quick_installer_single_server.sh provides a simple 1 step install of MariaDB ColumnStore bypassing the interactive wizard style interface and works for both root and non-root installs.

It will perform an install with these defaults:

- System-Name = columnstore-1
- Single-Server Install with 1 Performance Module
- Storage - Internal
- DBRoot - DBroot #1 assigned to Performance Module #1

### Running quick_installer_single_server.sh

```sql
# /usr/local/mariadb/columnstore/bin/quick_installer_single_server.sh
```

Now you are ready to use the system.

## Custom Installation of Single-Server ColumnStore System

If you choose not to do the quick install and chose to customize the various options of installations using a wizard, you may use MariaDB ColumnStore postConfigure script.

### Custom Install Wizard: postConfigure

The postConfigure script is a custom wizard to do the system(server and storage) configuration and setup.  
Before running the postConfigure script, you must have done [Preparing for ColumnStore Installation 1.2.x](/kb/en/preparing-for-columnstore-installation-12x/) steps. Once you have done these steps,  then run the postConfigure script from the single server where you are installing ColumnStore.

Run the script 'postConfigure' to complete the configuration and installation.

#### Running postConfigure as root user:

```sql
/usr/local/mariadb/columnstore/bin/postConfigure
```

#### Running postConfigure as non-root user:

```sql
export COLUMNSTORE_INSTALL_DIR=/home/guest/mariadb/columnstore
export LD_LIBRARY_PATH=/home/guest/mariadb/columnstore/lib:/home/guest/mariadb/columnstore/mysql/lib
/home/guest/mariadb/columnstore/bin/postConfigure -i /home/guest/mariadb/columnstore
```

### Common Installation Examples

Once you start postConfigure, it will give general instruction, then for each configuration would give you options to select from. Next we look at these options

#### Setup System Server Type Configuration

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

#### Setup High Availability Data Storage Mount Configuration

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

#### Performing Configuration Setup and MariaDB Columnstore Startup

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

#### MariaDB Columnstore Memory Configuration

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