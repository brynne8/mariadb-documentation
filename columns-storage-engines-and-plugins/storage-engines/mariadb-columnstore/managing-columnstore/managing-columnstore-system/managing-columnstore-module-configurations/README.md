# Managing ColumnStore Module Configurations

# Configuring modules

A Module ([UM](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-architecture/columnstore-user-module/) or [PM](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-architecture/columnstore-performance-module/)) can be added or removed from MariaDB ColumnStore.

## Before adding modules or DBRoots

- Ensure you have the user password or ssh-key for all servers that will be added.
- All the dependent packages have been installed.
- The server has the same OS and 'locale' as the current servers
- All Database Updates commands like DDL/DML and cpimports are suspend until the module or DBRoot is successfully added

### ColumnStore Cluster Test Tool

This tool can be running before doing installation on a single-server or multi-node installs. It will verify the setup of all servers that are going to be used in the Columnstore System.

[https://mariadb.com/kb/en/mariadb/mariadb-columnstore-cluster-test-tool](https://mariadb.com/kb/en/mariadb/mariadb-columnstore-cluster-test-tool)

### Module IDs

Modules in the system are identified by the "um" or "pm"  followed by a unique number - such as um1, pm1, pm2, pm3 etc.

The system assigns the module id as they are added

## Adding DBRoots

To add dbroots (storage files) into your system requires two operations: creating the physical dbroots and assigning them to a [Performance Module](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-architecture/columnstore-performance-module/).

NOTE: Anytime you add a DBRoot to an existing system, its recommended that you run the Redistribute Data command. This will re-balance the data onto the new DBRoot.

[https://mariadb.com/kb/en/mariadb/columnstore-redistribute-data/](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/managing-columnstore/managing-columnstore-system/columnstore-redistribute-data/)

### Creating the physical dbroot

The OAM addDbRoot command is used to create the physical storage (dbroot):

```sql
 # mcsadmin addDbroot numRoots
```

where numRoots is the total number of new dbroots to be added. The command will return the dbroot id’s created.

Example to add 2 additional dbroots to an already existing 2 dbroot system:

```sql
# mcsadmin adddbroot 2
adddbroot Mon Aug 26 15:00:38 2013
New DBRoot IDs added = 3, 4
```

After the <em>addDbroot</em> command, dbroots 3 and 4 have been created. You can see that they have been created by using the <em>getStorageConfig</em> command. Along with other information, towards the bottom of the output you will see information that reflects the additional, unassigned dbroots.

```sql
# mcsadmin  getstorageconfig
:
System Assigned DBRoot Count = 2
DBRoot IDs assigned to 'pm1' = 1
DBRoot IDs assigned to 'pm2' = 2
DBRoot IDs unassigned = 3, 4
:
```

### Assign DBRoots to Performance Module

After adding a DBRoot, it must be assigned to a [Performance Module](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-architecture/columnstore-performance-module/) before it can be used.  Use the `mcsadmin assignDbrootPmConfig` command to do so.
Note: The  [Performance Module](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-architecture/columnstore-performance-module/) that the DBRoot is being assigned to must to Manually Offline. The system must be in a STOPPED state

#### Syntax

```sql
# mcsadmin assignDbrootPmConfig dbrootid perfmod
```

where
dbrootid is the unassigned dbroot(s) to be assigned. You can assign multiple dbroots to a single Performance module with a comma separated list.
perfmoed is the Performance Module being assigned the dbroots.

Example #1 of assigning the 2 new dbroots to 2 separate PMs:

```sql
# mcsadmin  assignDbrootPmConfig 3 pm1
assigndbrootpmconfig Tue Aug 19 18:03:15 2015
DBRoot IDs assigned to 'pm1' = 1
Changes being applied
DBRoot IDs assigned to 'pm1' = 1, 3
Successfully Assigned DBRoots
REMINDER: Update the /etc/fstab on pm1 to include these dbroot mounts

# mcsadmin  assignDbrootPmConfig 4 pm2
assigndbrootpmconfig Tue Aug 19 18:03:18 2015
DBRoot IDs assigned to 'pm2' = 2
Changes being applied
DBRoot IDs assigned to 'pm2' = 2, 4
Successfully Assigned DBRoots
REMINDER: Update the /etc/fstab on pm2 to include these dbroot mounts
```

Example #2 of assigning the 2 new dbroots to 1 PM:

```sql
# mcsadmin  assignDbrootPmConfig 3,4 pm2
assigndbrootpmconfig Tue Aug 19 18:17:46 2015
DBRoot IDs assigned to 'pm2' = 2
Changes being applied
DBRoot IDs assigned to 'pm2' = 2, 3, 4
Successfully Assigned DBRoots
REMINDER: Update the /etc/fstab on pm2 to include these dbroot mounts
```

Once completed, start the system back up with the [startsystem](https://mariadb.com/kb/en/mariadb/mariadb-columnstore-system-operations/#starting-the-system-or-modules) command.

### Unassign DBRoots to Performance Module

You can also unassign DBRoot from a [Performance Module](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-architecture/columnstore-performance-module/).  Use the `mcsadmin unassignDbrootPmConfig` command to do so.

Note: The  [Performance Module](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-architecture/columnstore-performance-module/) that the DBRoot is being unassigned from must to Manually Offline. The system must be in a STOPPED state

Note: You can't unassigned DBROOT #1. It always has to be assigned to the Active Parent [Performance Module](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-architecture/columnstore-performance-module/).

#### Syntax

```sql
# mcsadmin unassignDbrootPmConfig dbrootid perfmod
```

## Moving DBRoots

Before moving a DBRoot from 1 module to another, the system must be in a STOPPED state. DBRoots cannot be moved while the system is active.

Only DBRoots that are configured as external or glusterFS can be moved from 1 pm to another.

NOTE: DBRoot #1 cant be moved from its assigned PM. It always needs to be assigned to the Active Parent OAM Module. The DBRM Controller Node Process runs on this module and it makes updates the DBRM Data on DBRoot #1.

### Syntax

```sql
#mcsadmin  movePMDbrootConfig [fromPM] [DBRoot] [toPM] and press Enter.
```

Example: This example moves DBRoot 6 from PM6 to PM 5

```sql
# mcsadmin  movePmDbrootConfig pm6 6 pm5
movepmdbrootconfig Wed Mar 28 11:22:22 2015
DBRoot IDs currently assigned to 'pm6' = 6
DBRoot IDs currently assigned to 'pm5' = 5
DBroot IDs being moved, please wait...
DBRoot IDs newly assigned to 'pm6' =
DBRoot IDs newly assigned to 'pm5' = 5, 6
```

Here is an example of moving dbroot 2 from pm2 to pm1. Run this command from the Parent OAM module. If these leaves pm2 without a dbroot assigned, that module will need to be disabled. System will not start up if there is a pm without a dbroot assigned to it.

```sql
# mcsadmin 
   stopsystem y
   movePmDbrootConfig pm2 2 pm1
   altersystem-disablemodule pm2 y
   startsystem
```

## Adding modules

Before a modules can be added, the system must be ACTIVE or OFFLINE. Once added, they can be placed in-service with the <em>alterSystem-Enable</em> command.

### addModule command

```sql
mcsadmin addModule module_type number_of_modules IP_address_or_host_name (separated by commas) user_password
```

For example, to add two Performance Modules with host names MYHST1 and MYHST2:

```sql
mcsadmin addModule pm 2 MYHST1,MYHST2 mypwd
```

ColumnStore.xml is updated to add new modules and the appropriate files are installed to the new modules. If the module addition fails, the mcsadmin displays an error message. Additional details are located in the MariaDB ColumnStore log files on the Performance Module #1.

NOTE: When adding Performance Modules with multiple NICs, you must add the host name for all NICs. If you do not, the add module process will fail with invalid parameters.

This example will add 1 [User Module](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-architecture/columnstore-performance-module/) into the system and will assign the ID to be the next available ID. So if you system has a um1 and um2, the new module will be 'um3'.

```sql
 # mcsadmin addModule um 1 hosthameUm3 'password'
```

This example will add um3 into the system. So you can also specify the name of the module to be added

```sql
 # mcsadmin addModule um3 hosthameUm3 'password'
```

This example will add 2 [Performance Modules](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-architecture/columnstore-performance-module/) into the system and will assign the ID to be the next available ID. So if you system has a pm1 and pm2, the new modules will be 'pm3' and 'pm4'

```sql
 # mcsadmin addModule pm 2 hosthamePm3,hosthamePm4 'password'
```

Once a module is added, it will be in a MAN_DISABLE state. This means its not part of the functional system at this time.

## Enabling Modules

If a new module has been added or if a module was previously disable, use this command to enable the module. The command alterSystem-EnableModule will restart the system as part of its functionality to bring the new module into use. So there will be some down time during this process.

```sql
#mcsadmin alterSystem-EnableModule pm3
```

NOTE: Enabling a [User Module](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-architecture/columnstore-performance-module/) will automatically bring it Active and it will be part of the running system.
NOTE: Enabling a [Permorance Module](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-architecture/columnstore-performance-module/) will ONLY be automatically brought into the system if it has a DBRoot assigned to it. If it doesn't, it will be in the MAN_OFFLINE state. So at this point, user would need to move or assign a DBRoot to it. Then it could be made ACTIVE with the startSystem command.

### Adding module to AWS EC2 instances.

To add a module to the system on an Amazon EC2 system, perform one of the following:

- To accept default module IDs, add multiple modules and have it automatically create the instances:

```sql
addModule module_type number_of_modules
```

An example to add two Performance Modules with defaulted instance names, type the following:

```sql
addModule pm 2
```

- To accept default module IDs, add multiple modules and have it install on existing Instances:

```sql
addModule module_type number_of_modules instance-ids
```

An example to add two Performance Modules with instance names id-1234890 and id-9876598

```sql
addModule pm 2 id-1234890,id-9876598
```

- To create IDs manually one at a time and have it automatically create the Instance

```sql
addModule module_ID
```

For example to add one Performance Module with a default instance name,
type the following:

```sql
addModule pm2
```

- To create IDs manually one at a time and have it install on existing Instance
addModule module_ID instance-i
For example to add one User Module number 2 with instance name id-100,
type the following:

```sql
addModule um2 id-100
```

## Removing modules

Modules can be removed from the system when they are no longer needed or in the event that they need to be taken offline for hardware updates. A module can be removed if it is [disabled](/kb/en/mariadb-columnstore-system-operations/#disabling-system-modules) or if the system is [stopped](/kb/en/mariadb-columnstore-system-operations/#stopping-the-system)

NOTE: You cannot remove the last um or pm module.
To remove a module

- To remove the last modules added to the system,

```sql
#mcsadmin removeModule module_type number_of_modules
```

For example, to remove two Performance Modules:

```sql
#mcsadmin removeModule pm 2
```

- To remove a specific module,

```sql
#mcsadmin removeModule module_ID
```

For example to remove one User Module with module ID UM1285

```sql
#mcsadmin removeModule um1285
```

NOTE: There is a known issue with the Delete User Module or Delete Combination Performance Module that leaves the MariaDB ColumnStore config file in a bad configuration to where the file needs to be edited. This is documented in the Troubleshooting guide.

[https://mariadb.com/kb/en/library/system-troubleshooting-mariadb-columnstore/#query-failure-messagequeueclient-setup-unknown-name-or-service](https://mariadb.com/kb/en/library/system-troubleshooting-mariadb-columnstore/#query-failure-messagequeueclient-setup-unknown-name-or-service)

## Example command to add a Performance Module  and DBRoot to a Active system

This is assuming PM3 and DBRoot #3 were added

```sql
#mcsadmin stopSystem
#mcsadmin addModule pm 1 hostnamePm3 'password'
#mcsadmin addDBroot 1
#mcsadmin alterSystem-EnableModule pm3
#mcsadmin assignDbrootPmConfig 3 pm3
#mcsadmin startSystem
```

If you decide you want to roll this back:

```sql
#mcsadmin stopSystem y
#mcsadmin unassignDBRootPMConfig 2 pm2
#mcsadmin removeModule pm2 y
#mcsadmin startSystem
#mcsadmin removeDBRoot 2
```

The above example requires a stopSystem to unassign the db root from the pm server. RemoveDBRoot can only be performed on a started system as it checks whether the db root has data or not. Many other permutations of commands are of course possible including moving a db root to another pm server.

## Example command to add a User Module a Active system

```sql
#mcsadmin addModule um 1 hostnameUm2 'password'
#mcsadmin alterSystem-EnableModule um2
```