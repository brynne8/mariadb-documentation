# Installing and Configuring a Multi Server ColumnStore System - 1.2.X

## Preparing to Install

Review the [Preparing for ColumnStore Installation 1.2.x](/kb/en/preparing-for-columnstore-installation-12x/) document and ensure that any necessary pre-requisites have been completed on all target servers for the ColumnStore cluster including installing the ColumnStore software packages.

## Validating Pre-Requisites are Complete

The [ColumnStore Cluster Tester Tool](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-getting-started/preparing-and-installing-mariadb-columnstore-11x/mariadb-columnstore-cluster-test-tool/) can be used to verify that the target servers are are setup correctly and report specific known errors that can cause failures or timeouts in the cluster setup scripts.

The tool should be run from the same server as the subsequent quick install and postConfigure scripts. With no arguments the script will only test the current server. Specify the other servers in the cluster using the <em>--ippaddr</em> argument to validate those servers are reachable and also configured correctly.

## MariaDB ColumnStore Multi-Server Quick Installer

The script <em>quick_installer_multi_server.sh</em> provides a simple 1 step install of MariaDB ColumnStore bypassing the interactive wizard style interface and works for both root and non-root installs.

The script has 4 parameters.

- --pm-ip-addresses=xxx.xxx.xxx.xxx,xxx.xxx.xxx.xxx : IP Addresses of PM nodes, specify current node IP as first value.
- --um-ip-addresses=xxx.xxx.xxx.xxx,xxx.xxx.xxx.xxx : IP Addresses of other UM nodes. optional if combined node install is desired.
- --dist-install : Optional override default to perform legacy distributed installation which also performs remote ColumnStore software installation.
- --system-name=&lt;name&gt; : ColumnStore System Name, defaults to 'columnstore-1'

The script will then perform an equivalent installation to running postConfigure with the arguments:
It will perform an install with these defaults:

- System-Name : Argument given or defaults to 'columnstore-1'
- Multi-Server Install
<ul start="1"><li>if only <em>--pm-ip-addresses</em> specified then <em>combined</em> install with number of IP Addresses nodes.
</li><li>if both <em>--pm-ip-addresses</em> and <em>--um-ip-addresses</em> specified then <em>seperate</em> install with PM IP's Performance Module Nodes and UM IP's User Module Nodes.
</li><li>Non distributed install, i.e. ColumnStore software must be pre-installed on the other nodes.
<ul start="1"><li>Legacy distributed install is used if <em>--dist-install</em> specified which will perform remote install of the ColumnStore software on other nodes. SSH Keys must be setup to allow passwordless login to the other nodes as the OS installation user.
</li></ul>
</li></ul>
- Storage : Internal
- DBRoot : 1 DBroot per 1 Performance Module
- Local Query is disabled on um/pm install
- MariaDB Replication is enabled

NOTE: The Multi-Server Quick Installer defaults to a Non-Distributed Install meaning the user is required to install the MariaDB ColumnStore on all nodes and starting the ColumnStore Server on all non-pm1 nodes before running the script.

### Example: 1 UM 1 PM Deployment as Root

```sql
# /usr/local/mariadb/columnstore/bin/quick_installer_multi_server.sh --um-ip-addresses=10.128.0.4 --pm-ip-addresses=10.128.0.3
```

### Example : 2 PM Combo Non Root Distributed Install

```sql
# /home/guest/mariadb/columnstore/bin/quick_installer_multi_server.sh --pm-ip-addresses=10.128.0.3,10.128.0.4 --dist-install
```

## MariaDB ColumnStore Custom Installation

If you choose not to do the quick install and chose to customize the various options of installations using a wizard, you may use  MariaDB ColumnStore postConfigure script. Please look at [Custom Installation of Multi-Server Cluster](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-getting-started/preparing-and-installing-mariadb-columnstore-12x/custom-installation-of-multi-server-columnstore-cluster/).