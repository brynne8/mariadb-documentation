# MariaDB ColumnStore Cluster Test Tool

## Introduction

MariaDB ColumnStore Cluster Test Tool is used to validate that the server(s)/node(s) are setup and configured correctly to allow a install of the MariaDB ColumnStore Product. It is packaged with the MariaDB ColumnStore package starting at 1.0.10 . It should be run after the "Preparing For ColumnStore Installation" steps are performed on the server(s)/node(s) that are being used for the MariaDB ColumnStore Product.

When to use the Tool:

- This script can be run from PM1 after the packages has been installed. It can be run on single-server installs and it will validate that the correct dependent packages are installed on the system.
- It can be run on PM1 on multi-server installs to validate the local PM1 and the other servers that will make up the cluster are configured correctly according to the setup guide.
- On multi-server installs that are going to be configured to support Server Failover, the script can be run on the other PM servers to validate that those nodes can access the PM1.

Here is a link to the "Preparing For ColumnStore Installation" document:

[https://mariadb.com/kb/en/mariadb/preparing-for-columnstore-installation](https://mariadb.com/kb/en/mariadb/preparing-for-columnstore-installation)

Here are the items that are tested/check during the running of this tool:

- Node Ping test - ping test to remote nodes
- Node SSH test -- SSH login test to remote nodes
- ColumnStore Port (8600-8630,8700,8800,3306) test - use 'nmap' to check connectivity on ColumnStore ports to remote Nodes
- OS version - Detect, Validate, and Compare OS version to make sure they match on remote nodes with local node
- Locale settings - Compare Locale setting to make sure they match on remote nodes with local node
- Firewall settings - Verify that certain firewalls are disabled on all nodes.
- Date/Time settings - Compare Date/Time settings to make sure they match on remote nodes with local node
- MariaDB port usage - Checks that he MariaDB default port of 3306 is not in use
- Dependent packages installed - Checks that all dependency packages are installed on all nodes
- For non-root user install - test permissions on /tmp and /dev/shm

This tool can be used on single-server installs also, not just clusters with multiple nodes
On single-server installs, the following items will be tested:

- Dependent packages installed
- For non-root user install - test permissions on /tmp and /dev/shm

## Tool Installation

To perform testing on a multi-node system, these 2 packages are required to be installed before the tool is run.

- expect
- nmap

MariaDB ColumnStore Cluster Test Tool is a tar package that consist of mutliple files. It can be  download from here:

## Tool Usage

Here is how to execute the tool. 
Example is showing a root install

```sql
cd /usr/local/mariadb/columnstore/bin
./columnstoreClusterTester.sh 'options'
```

Addition information from the tool can be obtained by running 'help'

```sql
# cd ClusterTesterTool
# ./columnstoreClusterTester.sh -h

This is the MariaDB ColumnStore Cluster System Test tool.

It will run a set of test to validate the setup of the MariaDB Columnstore system.
This can be run prior to running MariaDB ColumnStore 'postConfigure' tool to verify
servers/nodes are configured properly. It should be run as the user of the planned
install. Meaning if MariaDB ColumnStore is going to be installed as root user,
 then run from root user. Also the assumption is that the servers/node have be
setup based on the Preparing for ColumnStore Installation.
It should also be run on the server that is designated as Performance Module #1.

Items that are checked:
	Node Ping test
	Node SSH test
	ColumnStore Port test
	OS version
	Locale settings
	Firewall settings
        Date/Time Settings
        MariaDB port 3306 availability
	Dependent packages installed
        For non-root user install - test permissions on /tmp and /dev/shm

Usage: ./columnstoreClusterTester.sh [options]
OPTIONS:
   -h,--help			Help
   --ipaddr=[ipaddresses]	Remote Node IP Addresses, if not provide, will only check local node
   --os=[os]			Optional: Set OS Version (centos6, centos7, debian8, debian9, suse12, ubuntu16).
   --password=[password]	Provide a user password. (Default: ssh-keys setup will be assumed)
   -c,--continue		         Continue on failures
   --logfile=[filename]        Optional: Output results to a log file

NOTE: Dependent package : 'nmap' and 'expect' packages need to be installed locally
```

### Options:

'ipaddr' - This is the list of IP Addresses or hostname of the other server/nodes that make up the MariaDB ColumnStore Cluster. If this is not provide, just the Single-Server Local node tests will be performed. You don't need to provide the IP address/hostname of the local node. You can enter multiple using one of these formats:
--ipaddr=192.168.1.1,192.196.1.2
--ipaddr=serverum1,serverpm2

'os' - This os optional. Tool will determine the Local OS version and validate it againest the supported OS.

This is the list of support versions:

- centos6
- centos7
- debian8
- debian9
- suse12
- ubuntu16

'password' - The tool will log into the other server/nodes that make up the MariaDB ColumnStore Cluster to perform test. So the user password or ssh-key setup is required.

NOTE: If no 'password' is provided, the tool will assume ssh-keys are setup between the local node and the other server/nodes that make up the MariaDB ColumnStore Cluster.

'continue' - This option allows the user to run the tool to completion while reporting test/check failures. When this option is not used, then a prompt will be issued allowing the user to stop and fix the issue or continue.

Here is an example where an error is detected. When the 'continue' option is not specified, a prompt is given to the user to exit or continue the test once an error has been detected.

```sql
./columnstoreClusterTester.sh --ipaddr=x.x.x.x

*** This is the MariaDB Columnstore Cluster System test tool ***

** Run Ping access Test to remote nodes

x.x.x.x  Node Passed ping test

** Run SSH Login access Test to remote nodes

x.x.x.x  Node Passed SSH login test

** Run OS check - OS version needs to be the same on all nodes

Local Node OS Version : Linux Debian stretch/sid
x.x.x.x Node OS Version : Linux RedHat 6.9
Failed, x.x.x.x has a different OS than local node

Failure occurred, do you want to continue? (y,n) > n
```