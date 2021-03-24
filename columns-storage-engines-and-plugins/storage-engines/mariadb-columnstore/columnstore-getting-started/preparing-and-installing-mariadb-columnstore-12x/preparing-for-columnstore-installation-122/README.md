# Preparing for ColumnStore Installation - 1.2.2

### Prerequisite

With version 1.2.2 of MariaDB ColumnStore, there should be no version of MariaDB Server or MySQL installed on the OS before a MariaDB ColumnStore is installed on the system. If you have an installation of MariaDB Server or MySQL, uninstall it before proceeding.

#### Configuration preparation

Before installing MariaDB ColumnStore, there is some preparation necessary. You will need to determine the following. Please refer to the MariaDB ColumnStore Architecture Document for additional information.

- How many User Modules (UMs) will your system need?

- How many Performance Modules (PMs) will your system need?

- How much disk space will your system need?

- Would you have passwordless ssh access between PMs?

- Do you want to run the install as root or an unprivileged user?

#### OS information

MariaDB ColumnStore is certified to run on:

RHEL/CentOS v6, v7

Ubuntu 16.04 LTS, Ubuntu 18.04 LTS

Debian v8, v9

SUSE 12

but it should run on any recent Linux system.  Make sure all nodes are running the same OS.

Make sure the locale setting on all nodes is the same.

To set locale to en_US and UTf-8, as root run:

```sql
# localedef -i en_US -f UTF-8 en_US.UTF-8
```

#### System administration information

Information your system administrator must provide before installing MariaDB ColumnStore:

- The hostnames of each interface on each node (optional).

- The IP address of each interface on each node.

- The user and password ColumnStore will run as should be consistent across all nodes. MariaDB ColumnStore can be installed as root or an unprivileged user.

Example for 3 PM, 1UM system, these are the steps required to configure PM-1 for password-less ssh. The equivalent steps must be repeated on every PM in the system.
The equivalent steps must be repeated on every UM in the system if the MariaDB ColumnStore Data Replication feature is enabled during the install process.

```sql
[root@pm- 1 ~] $ ssh-keygen
[root@pm- 1 ~] $ ssh-copy-id -i ~/.ssh/id_rsa.pub pm-1
[root@pm- 1 ~] $ ssh-copy-id -i ~/.ssh/id_rsa.pub pm-2
[root@pm- 1 ~] $ ssh-copy-id -i ~/.ssh/id_rsa.pub pm-3
[root@pm- 1 ~] $ ssh-copy-id -i ~/.ssh/id_rsa.pub um-1
```

If you will be using ssh keys...

- and plan to enable Data Replication feature, make sure you can login between all UM nodes and all other UM nodes.
- and plan to enable the Local Query Feature, make sure you can login between all UM nodes and all PM nodes.
- and plan to enable the Data Redundancy feature, make sure you can login to PM1 from PM1, and between all UM nodes, without an additional prompt.  This is required for the Data Redundancy feature.

#### Network configuration

MariaDB ColumnStore is quite flexible regarding networking. 
Some options are as follows:

- The interconnect between UMs and PMs can be one or more private VLANs. 
In this case MariaDB ColumnStore will automatically trunk the individual LANs together to provide greater effective bandwidth between the UMs and PMs.

- The PMs do not require a public LAN access as they only need to communicate with the UMs, unless the local query feature is desired.

- The UMs most likely require at least one public interface to access the MySQL server front end from the site LAN. This interface can be a separate physical or logical connection from the PM interconnect.

- You can use whatever security your site requires on the public access to the MySQL server front end on the UMs. By default it is listening on port 3306.

- MariaDB ColumnStore software only requires a TCP/IP stack to be present to function. You can use any physical layer you desire.

#### MariaDB ColumnStore port usage

The MariaDB ColumnStore daemon uses port 3306.

You must reserve the following ports to run the MariaDB ColumnStore
software:
8600 - 8630, 8700, and 8800

#### Installation and Data Directory Paths

MariaDB Columnstore requires that the software gets installed and the data paths in the configuration files don't get changed from their initial settings. The system will not function properly if they are changed or the software is install in the incorrect places.

For root user install, it is required that it gets installed and it runs in this location:

/usr/local/

For non-root user install, it is required that it gets installed and it runs in this location:

/home/'non-root-user-name'

Example if installed and running as 'mysq' user, is has to be installed here:

/home/mysql/

IMPORTANT: Don't change any of the configuration files, Columnstore.xml or my.cnf, to point to a different install or data location.

If you need to have the software or the data (dbroot backend data or the mysql data) placed in a different directory than what is required above, you can use soft-links.

To install in a different directory for root user, here is an example of setting up a softlink

```sql
# ln -s /mnt/columnstore /usr/local/mariadb/columnstore
```

To install in a different directory for non-root user, here is an example of setting up a softlink

```sql
# ln -s /mnt/columnstore /home/mysql/mariadb/columnstore
```

To have the DBRoot data1 placed in a different directory, here is an example of setting up a softlink

```sql
# ln -s /mnt/data1 /usr/local/mariadb/columnstore/data1
```

To have the MariaDB/MySQL aata placed in a different directory, here is an example of setting up a softlink

```sql
# ln -s /mnt/mysql /usr/local/mariadb/columnstore/mysql/db
```

If you do setup softlinks, make sure the permissions and users are setup for the new directories that match the current directories.

#### Storage and Database files (DBRoots)

'DBRoot' is a term used by ColumnStore to refer to a storage unit, located at mariadb/columnstore/data&lt;N&gt;, where N is the identifier for that storage unit.  You will see that term throughout the documentation. The name of this location can't be changed. But you can use soft-links to point it to other locations.

MariaDB ColumnStore supports many storage options.

- Internal - Internal means you will use block storage already mounted and available on each node.  The data directories (mariadb/columnstore/dataN) can be links to other locations if preferable.  With Internal storage setup, there is no High Availability Performance Module Failover capability. This is one of the limitations to using Internal storage. Its also for systems that have SAN that are mounted to single Performance Module. Its not shared mounted to all Performance Modules.

- External - External means the SAN devices used for the DBRoots are shared mounted to all Performance Modules configured in the system. With External storage setup, the High Availability Performance Module Failover capability is supported, because mounts can be moved quickly from one node to another. So this is the only time that 'External' storage should be configured.

- Data Redundancy - MariaDB ColumnStore supports Data Redundancy for both internal and external configurations.  For Data Redundancy to be an option, the user is required to install the third party GlusterFS software on all Performance Modules. This is discussed later in the Package Dependency section. With Data Redundancy configured, High Availability Performance Module Failover capabilities is supported.

IMPORTANT: When using external storage, be sure that only whole DBRoots are on the external filesystem.  Do not use higher level directories, which may include the ColumnStore binaries and libraries.  For example, /usr/local/mariadb/columnstore/data1 (aka DBRoot 1) would be a good candidate for external storage, but /usr/local/mariadb/columnstore would not be.

#### Local database files

If you are using internal storage, the DBRoot directories will be created under the installation directory, for example /usr/local/mariadb/columnstore/data&lt;N&gt; where N is the DBRoot number. 
There is no performance gain from assigning more than one DBRoot to a single storage device.

DBRoots can be linked to other locations with more capacity if necessary.

#### SAN mounted database files

If you are using a SAN for storage, the following must be taken into account:

- Each DBRoot must be a separate filesystem.

- Each DBRoot should have the same capacity and performance characteristics, and each Performance Module should have the same number of DBRoots assigned for best performance.

- MariaDB ColumnStore performs best on filesystems with low overhead, such as the ext family, however it can use any Linux filesystem.  ColumnStore writes relatively large files (up to 64MB) compared to other databases, so please take that into account when choosing and configuring your filesystems.

- For the failover feature to work, all Performance Modules must be able to mount any DBRoot.

- The fstab file on all Performance Modules must include all of the external mounts, all with the 'noauto' option specified.  ColumnStore will manage the mounting and unmounting.

These lines are an example of an installation with 2 DBRoots configured.

```sql
/dev/sda1 /usr/local/mariadb/columnstore/data1 ext2 noatime,nodiratime,noauto 0 0
/dev/sdd1 /usr/local/mariadb/columnstore/data2 ext2 noatime,nodiratime,noauto 0 0
```

#### Performance optimization considerations

There are optimizations that should be made when using MariaDB ColumnStore listed below. As always, please consult with your network administrator for additional optimization considerations for your specific installation needs.

#### GbE NIC settings:

- Modify /etc/rc.d/rc.local to include the following:

```sql
/sbin/ifconfig eth0 txqueuelen 10000
```

- Modify /etc/sysctl.conf for the following:

```sql
# increase TCP max buffer size
net.core.rmem_max = 16777216
net.core.wmem_max = 16777216

# increase Linux autotuning TCP buffer limits
# min, default, and max number of bytes to use
net.ipv4.tcp_rmem = 4096 87380 16777216
net.ipv4.tcp_wmem = 4096 65536 16777216

# don't cache ssthresh from previous connection
net.ipv4.tcp_no_metrics_save = 1

# recommended to increase this for 1000 BT or higher
net.core.netdev_max_backlog = 2500

# for 10 GigE, use this
net.core.netdev_max_backlog = 30000
```

NOTE: Make sure there is only 1 setting of net.core.netdev_max_backlog in the /etc/sysctl.conf file.

- Cache memory settings:
The kernel parameter vm.vfs_cache_pressure can be set to a lower value than 100 to attempt to retain caches for inode and directory structures. This will help improve read performance. A value of 10 is suggested. The following commands must be run as the root user or with sudo.

To check the current  value:

```sql
cat /proc/sys/vm/vfs_cache_pressure
```

To set the current value until the next reboot:

```sql
sysctl -w vm.vfs_cache_pressure=10
```

To set the value permanently across reboots, add the following to /etc/sysctl.conf:

```sql
vm.vfs_cache_pressure = 10
```

#### System settings considerations

##### umask setting

The default setting of 022 is what is recommended.  ColumnStore requires that it not end with a 7, like 077. 
For example, on a root installation, mysqld runs as the mysql user, and needs to be able to read the ColumnStore configuration file, which likely is not owned by mysql.  If it were, then the other ColumnStore processes could not read it.

The current umask can be determined by running the 'umask' command:
A value of 022 can be set in the current session or in /etc/profile with the command: <br>

```sql
umask 022
```

#### Firewall considerations

The MariaDB ColumnStore uses TCP ports 3306, 8600 - 8630, 8700, and 8800.  Port 3306 is the default port for clients to connect to the MariaDB server; the others are for internal communication and will need to be open between ColumnStore nodes.

To disable any local firewalls and SELinux on mutli-node installations, as root do the following.

CentOS 6 and systems using iptables

```sql
#service iptables save  (Will save your existing IPTable Rules)
#service iptables stop  (It will disable firewall Temporary)
```

To Disable it Permanentely:

```sql
#chkconfig iptables off
```

CentOS 7 and systems using systemctl with firewalld

```sql
#systemctl status firewalld  
#systemctl stop firewalld  
#systemctl disable firewalld  
```

Ubuntu and systems using ufw

```sql
#service ufw stop  (It will disable firewall Temporary)
```

To Disable it Permanently:

```sql
#chkconfig ufw off
```

SUSE

```sql
#/sbin/rcSuSEfirewall2 status
#/sbin/rcSuSEfirewall2 stop 
```

To disable SELinux, edit "/etc/selinux/config" and find line:

```sql
SELINUX=enforcing
```

Now replace it with,

```sql
SELINUX=disabled
```

#### User considerations

MariaDB ColumnStore can be installed in different user configurations.

#### Root User installation and execution

MariaDB ColumnStore is installed by root and the MariaDB ColumnStore commands are run by root.
With this installation type, you have the option of install using rpm, deb or binary packages.  You can install the packages with the standard tools yum, apt, or zypper.

The default package installation directory is /usr/local/mariadb/columnstore
The location of the temporary files created by MariaDB ColumnStore is /tmp/columnstore_tmp.

#### Unprivileged user installation and execution

MariaDB ColumnStore is installed by an unprivileged user and the MariaDB ColumnStore commands are run by that user. With this installation type, you are required to install using the binary tarball, which can be downloaded from the MariaDB ColumnStore Downloads page: [https://mariadb.com/downloads/#mariadbax](https://mariadb.com/downloads/#mariadbax)

The default package installation directory is the home directory of the user account.  The location of the temporary files created by MariaDB ColumnStore is $HOME/.tmp

#### Root user installation and unprivileged user execution

MariaDB ColumnStore is installed by root user and the MariaDB ColumnStore commands are run by an unprivileged user. This document will show how to configure the system to run commands as an unprivileged user:  [https://mariadb.com/kb/en/library/mariadb-columnstore-system-usage/#non-root-user-mariadb-columnstore-admin-console](https://mariadb.com/kb/en/library/mariadb-columnstore-system-usage/#non-root-user-mariadb-columnstore-admin-console)

With this installation type, you have the option of install using rpm, deb or binary packages, which you can install with the standard tools yum, apt, zypper.

The default package installation directory is /usr/local/mariadb/columnstore
The location of the temporary files created by MariaDB ColumnStore is /tmp/columnstore_tmp.

### Package dependencies

#### Boost libraries

MariaDB ColumnStore requires that the boost package of 1.53 or newer is installed.

For Centos 7, Ubuntu 16, Debian 8, SUSE 12 and other newer OS's, you can just install the default boost package that comes with your distribution.

```sql
# yum -y install boost
```

or

```sql
# apt-get -y install libboost-all-dev
```

For SUSE 12, you will need to install the boost-devel package, which is part of the SLE-SDK package.

```sql
SUSEConnect -p sle-sdk/12.2/x86_64
zypper install boost-devel
```

For CentOS 6, you can either download and install the MariaDB Columnstore Centos 6 boost library package or build version 1.55 from source and deploy it to each ColumnStore node.

To download the pre-built libraries from MariaDB, go to binaries download page and download "centos6_boost_1_55.tar.gz"

[https://mariadb.com/downloads/columnstore](https://mariadb.com/downloads/columnstore)

Click All Versions - &gt; 1.0.x -&gt; centos -&gt; x86_64

Install package on each server in the cluster.

```sql
wget https://downloads.mariadb.com/ColumnStore/1.0.14/centos/x86_64/centos6_boost_1_55.tar.gz
tar xfz centos6_boost_1_55.tar.gz -C /usr/lib
ldconfig
```

Downloading and build the boost libraries from source:

NOTE: This requires that the "Development Tools" group and cmake be installed prior to this.

```sql
yum groupinstall "Development Tools"
yum install cmake
```

Here is the procedure to download and build the boost source.  As root do the following:

```sql
cd /usr/
wget http://sourceforge.net/projects/boost/files/boost/1.55.0/boost_1_55_0.tar.gz
tar zxvf boost_1_55_0.tar.gz
cd boost_1_55_0
./bootstrap.sh --with-libraries=atomic,date_time,exception,filesystem,iostreams,locale,program_options,regex,signals,system,test,thread,timer,log --prefix=/usr
./b2 install
ldconfig
```

#### Other packages

Additional packages are required on each ColumnStore node.

##### Centos 6/7

```sql
# yum -y install expect perl perl-DBI openssl zlib file sudo libaio rsync snappy net-tools numactl-libs nmap
```

##### Ubuntu 16/18

```sql
# apt-get -y install tzdata libtcl8.6 expect perl openssl file sudo libdbi-perl libboost-all-dev libreadline-dev rsync libsnappy1v5 net-tools libnuma1 nmap
```

##### Debian 8

```sql
# apt-get install expect perl openssl file sudo libdbi-perl libboost-all-dev libreadline-dev rsync libsnappy1 net-tools libnuma1 nmap
```

##### Debian 9

```sql
# apt-get install expect perl openssl file sudo libdbi-perl libboost-all-dev libreadline-dev rsync  net-tools libsnappy1v5 libreadline5 libaio1 libnuma1 nmap

```

##### SUSE 12

```sql
zypper install expect perl perl-DBI openssl file sudo libaio1 rsync net-tools libsnappy1 libnuma1 nmap
```

#### Data Redundancy packages

To enable the Data Redundancy feature, install and enable GlusterFS version 3.3.1 or higher on each Performance Module.

##### Centos 6/7

```sql
# yum -y install centos-release-gluster
# yum -y install glusterfs glusterfs-fuse glusterfs-server

start / enable service: 
# systemctl enable glusterd.service
# systemctl start glusterd.service 
```

##### Ubuntu 16/18

```sql
# apt-get install glusterfs-server attr

start / enable service:
# systemctl start glusterfs-server.service 
# systemctl enable glusterfs-server.service 
```

##### Debian 8

```sql
# apt-get -y install glusterfs-server attr

start / enable service:
# update-rc.d glusterfs-server defaults
```

##### Debian 9

```sql
# apt-get install glusterfs-server attr

start / enable service:
# systemctl start glusterfs-server.service 
# systemctl enable glusterfs-server.service 
```

##### SUSE 12

```sql
# zypper install glusterfs

start / enable service:
# chkconfig glusterd on
```

#### System Logging Package

MariaDB ColumnStore can use the following standard logging systems.

- syslog
- rsyslog
- syslog-ng

### Download location

The standard ColumnStore RPM and DEB packages are available at [https://mariadb.com/downloads/columnstore](https://mariadb.com/downloads/columnstore).  If you need to install ColumnStore without root-level permissions, you must use the binary tarball, which is available at [https://downloads.mariadb.com/ColumnStore/](https://downloads.mariadb.com/ColumnStore/)

Note: MariaDB ColumnStore will configure a root user with no password in the MariaDB server initially.  Afterward, you may setup users and permissions within MariaDB as you normally would.

### Choosing the type of initial download/install

#### Non-Distributed Installation

The default MariaDB ColumnStore installation process supports a Non-Distributed installation mode for multi-node deployments. With this option, the user will need to install the ColumnStore packages manually on each node.

This option would be used when Package Repo install of rpm/deb packages are the preferred method to install MariaDB ColumnStore. Its also used for MariaDB ColumnStore Docker images.

The Pre-install setup time will take longer with the Non-Distributed Installation, but the system Install and startup via the install app 'postConfigure' will be faster.

#### Distributed Install

With the Distributed option, MariaDB ColumnStore will distribute and install the packages automatically on each node.  The user installs ColumnStore on Performance Module 1, then during the postConfigure step, ColumnStore will handle the rest.

This option would be used on system with multi-nodes where the package only had to be installed on 1 node, the Performance Module 1, the app 'postConfigure' would distribute and all down to all of the other nodes.

The Pre-install setup time would be shorter with the Non-Distributed Installation, but the system Install and startup via the install app 'postConfigure' will be take longer.

### Root user installations

#### Initial download/install of MariaDB ColumnStore Packages

The MariaDB ColumnStore RPM and DEB Packages can be installed using yum/apt-get commands.
Check here for additional Information:

[https://mariadb.com/kb/en/library/installing-mariadb-ax-from-the-package-repositories](https://mariadb.com/kb/en/library/installing-mariadb-ax-from-the-package-repositories)

#### Initial download/install of MariaDB ColumnStore Package with the AX package

The MariaDB ColumnStore RPM, DEB and binary packages can be installed along with the other packages that make up the AX package.
Check here for additional Information:

[https://mariadb.com/kb/en/library/columnstore-getting-started-installing-mariadb-ax-from-the-mariadb-download/](https://mariadb.com/kb/en/library/columnstore-getting-started-installing-mariadb-ax-from-the-mariadb-download/)

#### Initial download &amp; installation of MariaDB ColumnStore RPMs

1 Install MariaDB ColumnStore as user root:

Note: MariaDB ColumnStore will configure a root user with no password in the MariaDB server initially.  Afterward, you may setup users and permissions within MariaDB as you normally would.

Note: The packages will be installed at /usr/local. This is required for root user installations.

- Download the appropriate ColumnStore package and place in the /root directory.
- Unpack the tarball, which contains multiple RPMs.
- Install the RPMs. The MariaDB ColumnStore software will be installed in /usr/local/. <br>
<code class="fixed" style="white-space:pre-wrap">rpm -ivh mariadb-columnstore*release#*.rpm</code>
<br>

#### Initial download &amp; installation of MariaDB ColumnStore binary package

The MariaDB ColumnStore binary packages can be downloaded from [https://downloads.mariadb.com/ColumnStore/](https://downloads.mariadb.com/ColumnStore/).  Select version 1.2.2, then navigate to the appropriate package for your system.  The binary package will have the extension '.bin.tar.gz' rather than '.rpm.tar.gz' or '.deb.tar.gz'.

- Unpack the tarball in /usr/local/ <br>
<code class="fixed" style="white-space:pre-wrap">tar -zxf mariadb-columnstore-release#.x86_64.bin.tar.gz</code>

Run the post-install script: <br>

```sql
/usr/local/mariadb/columnstore/bin/post-install
```

#### Initial download &amp; installation of MariaDB ColumnStore DEB package

1 Download the package mariadb-columnstore-release#.amd64.deb.tar.gz into the /root directory of the server where you are installing MariaDB ColumnStore.
2 Unpack the tarball, which contains multiple DEBs. <br>
<code class="fixed" style="white-space:pre-wrap"> tar -zxf  mariadb-columnstore-release#.amd64.deb.tar.gz</code> <br>
3 Install the MariaDB ColumnStore DEBs. The MariaDB ColumnStore software will be installed in /usr/
local/. <br>
<code class="fixed" style="white-space:pre-wrap"> dpkg -i mariadb-columnstore*release#*.deb</code>

### Unprivileged user installation

MariaDB Columnstore is installed to run as an unprivileged user using the binary package. It will need to be run on all MariaDB ColumnStore nodes.

For the purpose of these instructions, the following assumptions are:

- Unprivileged user 'mysql' is used in this example
- Installation directory is /home/mysql/mariadb/columnstore

Tasks involved:

- Create the mysql user and group of the same name (by root user)
- Update sudo configuration, if needed (by root user)
- Set the user file limits (by root user)
- Modify fstab if using 'external' storage (by root user)
- Uninstall existing MariaDB Columnstore installation if needed (by root user)
- Update permissions on certain directories that MariaDB Columnstore writes (by root user)
- MariaDB Columnstore Installation (by the new user user)
- Enable MariaDB Columnstore to start automatically at boot time

#### Creation of a new user (by root user)

Before beginning the binary tar file installation you will need your system administrator to set up accounts
for you on every MariaDB Columnstore node. The account name &amp; password must be the same on every node. If you change the password on one node, you must change it on every node. The user ID must be the same on every node as well. In the examples below we will use the account name 'mysql' and the password 'mariadb'.

- create new user

Group ID is an example, can be different than 1000, but needs to be the same on all servers in the cluster

<code class="fixed" style="white-space:pre-wrap">adduser mysql -u 1000</code>

- create group

```sql
addgroup mysql
moduser -g mysql mysql
```

The value for user-id must be the same for all nodes.

- Assign a password to newly created user

<code class="fixed" style="white-space:pre-wrap">passwd mysql</code>

- Log in as user mysql

<code class="fixed" style="white-space:pre-wrap">su - mysql</code>

IMPORTANT: It is required that the installation directory will be the home directory of the user account. So it would be '/home/mysql/' in this example.  The installation directory must be the same on every node.  In the examples below we will use the path '/home/mysql/mariadb/columnstore'.

#### Update sudo configuration, if needed  (by root user)

In the MariaDB ColumnStore 1.2.1 and later, sudo configuration is only required for certain system configurations.

These would include:

- Data Redundancy
- External Storage
- Amazon EC2 using EBS storage

If your system is not configured with any of these, you can skip this section.

#### sudo configuration for Data Redundancy

The sudo configuration on each node will need to allow the new user to run the gluster, mount, umount, and chmod commands.  Add the following lines to your /etc/sudoers file, changing the paths to those commands if necessary on your system.

```sql
mysql  ALL=NOPASSWD: /sbin/gluster
mysql ALL=NOPASSWD: /usr/bin/mount
mysql ALL=NOPASSWD: /usr/bin/umount
mysql ALL=NOPASSWD: /usr/bin/chmod
```

Comment the following line, which will allow the user to login without a terminal:

```sql
#Defaults requiretty
```

#### sudo configuration for External Storage

The sudo configuration on each node will need to allow the new user to run the chmod, chown, mount, and umount commands.  Add the following lines to your /etc/sudoers file, changing the path of each command if necessary on your system.

```sql
mysql ALL=NOPASSWD: /usr/bin/chmod
mysql ALL=NOPASSWD: /usr/bin/chown
mysql ALL=NOPASSWD: /usr/bin/mount
mysql ALL=NOPASSWD: /usr/bin/umount
```

Comment the following line, which will allow the user to login without a terminal:

```sql
#Defaults requiretty
```

#### sudo configuration for Amazon EC2 using EBS storage

The sudo configuration on each node will need to allow the new user to run several commands.  Add the following lines to your /etc/sudoers file, changing the path of each command if necessary on your system.

```sql
mysql ALL=NOPASSWD: /usr/sbin/mkfs.ext2
mysql ALL=NOPASSWD: /usr/bin/chmod
mysql ALL=NOPASSWD: /usr/bin/chown
mysql ALL=NOPASSWD: /usr/bin/sed
mysql ALL=NOPASSWD: /usr/bin/mount
mysql ALL=NOPASSWD: /usr/bin/umount
```

Comment the following line, which will allow the user to login without a terminal:

```sql
#Defaults requiretty
```

#### Set the user file limits (by root user)

ColumnStore needs the open file limit to be increased for the new user. To do this edit the /etc/security/limits.conf file and make the following additions at the end of the file:

```sql
mysql hard nofile 65536
mysql soft nofile 65536
```

If you are already logged in as 'mysql' you will need to logout, then login for this change to take effect.

#### Modify fstab if using the external storage option (by root user)

If you are using an 'external' storage configuration, you will need to allow the new user to mount and unmount those filesystems.  Add the 'users' option to those filesystems in your /etc/fstab file.

<br>Example entries: <br>
/dev/sda1 /home/mysql/mariadb/columnstore/data1 ext2 noatime,nodiratime,noauto,users 0 0 <br>
/dev/sdd1 /home/mariadb/columnstore/data2 ext2 noatime,nodiratime,noauto,users 0 0 <br>

The external filesystems will need to be owned by the new user. This is an
example command run as root setting the owner and group of dbroot /dev/sda1 to the mysql user:

```sql
mount /dev/sda1 /tmpdir 
chown -R mysql:mysql /tmpdir 
umount /tmpdir 
```

#### Update permissions on directories that MariaDB Columnstore writes to (by root user)

ColumnStore uses POSIX shared memory segments.  On some systems, the entry point /dev/shm does not allow regular users to create new segments.  If necessary, permissions on /dev/shm should be set such that the new user or group has full access.  The default on most systems is 777.

```sql
chmod 777 /dev/shm
```

For Data Redundancy using GlusterFS configured system:

```sql
chmod 777 -R /var/log/glusterfs
```

To allow external storage on an Amazon EC2 instance using EBS storage:

```sql
chmod 644 /etc/fstab
```

#### Uninstall existing MariaDB Columnstore installation, if needed (by root user)

If MariaDB Columnstore has ever before been installed on any of the planned hosts as a root user install, you must have the system administrator verify that no remnants of that installation exist. The unprivileged installation will not be successful if there are MariaDB Columnstore files owned by root on any of the hosts.

- Verify the MariaDB Columnstore installation directory does not exist: <br>
The /usr/local/mariadb/columnstore directory should not exist at all unless it is your target directory, in
which case it must be completely empty and owned by the new user.

- Verify the /etc/fstab entries are correct for the new installation. <br>

- Verify the /etc/default/columnstore directory does not exist.<br>

- Verify the /var/lock/subsys/mysql-Columnstore file does not exist.<br>

- Verify the /tmp/StopColumnstore file does not exist. <br>

#### MariaDB Columnstore installation (by the new user)

You should be familiar with the general MariaDB Columnstore installation instructions in this guide as you will be asked the same questions during installation.

If performing the distributed-mode installation across multiple node, the binary package only needs to be installed on Performance Module #1 (pm1).  If performing a non-distributed installation, the binary package is required to install the MariaDB ColumnStore on all the nodes in the system.

- Log in as the unprivileged user (mysql, in our example)
Note: Ensure you are at your home directory before proceeding to the next step

- Now place the MariaDB Columnstore binary tar file in your home directory on the host you will be using as
PM1. Untar the binary distribution package to the /home/mysql directory:
tar -xf mariadb-columnstore-release#.x86_64.bin.tar.gz

- Run post installation:

If performing the distributed-mode installation, post-install only needs to be run on Performance Module #1 (pm1).  If performing non-distributed installations, post-install needs to be run on all the nodes in the system.

NOTE: $HOME is the same as /home/mysql in this example

```sql
./mariadb/columnstore/bin/post-install --installdir=$HOME/mariadb/columnstore
```

Here is an example of what is printed by the post-install command

```sql
NOTE: For non-root install, you will need to run the following commands as root user to
      setup the MariaDB ColumnStore System Logging

export COLUMNSTORE_INSTALL_DIR=/home/mysql/mariadb/columnstore
export LD_LIBRARY_PATH=:/home/mysql/mariadb/columnstore/lib:/home/mysql/mariadb/columnstore/mysql/lib:/home/mysql/mariadb/columnstore/lib:/home/mysql/mariadb/columnstore/mysql/lib
/home/mysql/mariadb/columnstore/bin/syslogSetup.sh --installdir=/home/mysql/mariadb/columnstore --user=mysql install
 
The next steps are:

If installing on a pm1 node using non-distributed install

export COLUMNSTORE_INSTALL_DIR=/home/mysql/mariadb/columnstore
export LD_LIBRARY_PATH=:/home/mysql/mariadb/columnstore/lib:/home/mysql/mariadb/columnstore/mysql/lib:/home/mysql/mariadb/columnstore/lib:/home/mysql/mariadb/columnstore/mysql/lib
/home/mysql/mariadb/columnstore/bin/postConfigure -i /home/mysql/mariadb/columnstore

If installing on a pm1 node using distributed install

export COLUMNSTORE_INSTALL_DIR=/home/mysql/mariadb/columnstore
export LD_LIBRARY_PATH=:/home/mysql/mariadb/columnstore/lib:/home/mysql/mariadb/columnstore/mysql/lib:/home/mysql/mariadb/columnstore/lib:/home/mysql/mariadb/columnstore/mysql/lib
/home/mysql/mariadb/columnstore/bin/postConfigure -i /home/mysql/mariadb/columnstore -d

If installing on a non-pm1 using the non-distributed option:

export COLUMNSTORE_INSTALL_DIR=/home/mysql/mariadb/columnstore
export LD_LIBRARY_PATH=:/home/mysql/mariadb/columnstore/lib:/home/mysql/mariadb/columnstore/mysql/lib:/home/mysql/mariadb/columnstore/lib:/home/mysql/mariadb/columnstore/mysql/lib
/home/mysql/mariadb/columnstore/bin/columnstore start
```

- Setup System Logging

For an unprivileged user installation, you will need to run the following commands as root on all nodes after the post-install command is run to make log entries go to the right places:

```sql
export COLUMNSTORE_INSTALL_DIR=/home/mysql/mariadb/columnstore
export LD_LIBRARY_PATH=/home/mysql/mariadb/columnstore/lib:/home/mysql/mariadb/columnstore/mysql/lib
/home/mysql/mariadb/columnstore/bin/syslogSetup.sh --installdir=/home/mysql/mariadb/columnstore --user=mysql install
```

Without that, log entries will go to the main system log file, and low-severity logs may not be recorded at all.

- Start MariaDB ColumnStore Service for Non-Distributed installations

If performing the non-distributed installation mode, the 'columnstore' service needs to be started on all nodes in the system except Performance Module #1 (pm1). postConfigure is on pm1, which is documented in the next section.

Starting the 'columnstore' service:

```sql
export COLUMNSTORE_INSTALL_DIR=/home/mysql/mariadb/columnstore
export LD_LIBRARY_PATH=/home/mysql/mariadb/columnstore/lib:/home/mysql/mariadb/columnstore/mysql/lib
/home/mysql/mariadb/columnstore/bin/columnstore start
```

- Running ColumnStore Configuration and Installation Tool, 'postConfigure'

On PM1, run the 2 export commands that were printed by the post-install command, then run postConfigure, which would
look like the following:

```sql
export COLUMNSTORE_INSTALL_DIR=/home/mysql/mariadb/columnstore
export LD_LIBRARY_PATH=/home/mysql/mariadb/columnstore/lib:/home/mysql/mariadb/columnstore/mysql/lib
/home/mysql/mariadb/columnstore/bin/postConfigure -i /home/mysql/mariadb/columnstore
```

#### Configure ColumnStore to run automatically at boot

To configure MariaDB Columnstore unprivileged installation to start automatically at boot time, perform the following steps on each Columnstore node:

- Add the following to your local startup script (/etc/rc.local or similar), modifying as necessary:

```sql
runuser -l mysql -c "/home/mysql/mariadb/columnstore/bin/columnstore start"
```

Note: Make sure the above entry is added to the rc.local file that gets executed at boot time.
Depending on the OS installation, rc.local could be in a different location.

- MariaDB Columnstore will setup and log using your current system logging application in the directory
/var/log/mariadb/columnstore.

### MariaDB Server Password setup

If you plan to setup a root user password in the MariaDB server Database, remember to setup the Password Configuration file on each User Module and Performance Module that has User Modules functionality.

[https://mariadb.com/kb/en/library/mariadb-columnstore-system-usage/#logging-into-mariadb-columnstore-mariadb-console](https://mariadb.com/kb/en/library/mariadb-columnstore-system-usage/#logging-into-mariadb-columnstore-mariadb-console)

### ColumnStore Cluster Test Tool

This tool can be run before installation. It will verify the setup of all servers that are going to be used in the Columnstore System.

[https://mariadb.com/kb/en/mariadb/mariadb-columnstore-cluster-test-tool](https://mariadb.com/kb/en/mariadb/mariadb-columnstore-cluster-test-tool)

### How to Configuration and Launch MariaDB Columnstore

#### ColumnStore Quick Launch Tools

There are 3 Quick Launch Tools that can be used to launch basic systems. These are 1 step command tool where you provide the number of modules or amazon instances and the tool will take care of the rest.

1. Single Server Quick Launch

[https://mariadb.com/kb/en/library/installing-and-configuring-a-single-server-columnstore-system-12x/#mariadb-columnstore-quick-installer-for-a-single-server-system](https://mariadb.com/kb/en/library/installing-and-configuring-a-single-server-columnstore-system-12x/#mariadb-columnstore-quick-installer-for-a-single-server-system)

2. Multi Server Quick Launch

[https://mariadb.com/kb/en/library/installing-and-configuring-a-multi-server-columnstore-system-12x/#mariadb-columnstore-quick-installer](https://mariadb.com/kb/en/library/installing-and-configuring-a-multi-server-columnstore-system-12x/#mariadb-columnstore-quick-installer)

3. Amazon AMI Quick Launch

[https://mariadb.com/kb/en/library/installing-and-configuring-a-columnstore-system-using-the-amazon-ami/#mariadb-columnstore-one-step-quick-installer-script-quick_installer_amazonsh](https://mariadb.com/kb/en/library/installing-and-configuring-a-columnstore-system-using-the-amazon-ami/#mariadb-columnstore-one-step-quick-installer-script-quick_installer_amazonsh)

#### ColumnStore Configuration and Installation Tool

MariaDB Columnstore System Configuration and Installation tool, 'postConfigure', will Configure the MariaDB Columnstore System and will perform a Package Installation of all of the Servers within the System that is being configured. It will prompt the user to for information like IP addresses, storage types, MariaDB Server, and system features. At the end, it will start the ColumnStore system.

NOTE:  When prompted for password, enter the non-user account password OR just hit enter if you
have configured password-less ssh keys on all nodes (Please see the “System
Administration Information” section earlier in this guide for more information on ssh keys.)

This tool is always run on the Performance Module #1.

Example uses of this script are shown in the Single and Multi Server Installations Guides.

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


Usage: postConfigure [-h][-c][-u][-p][-qs][-qm][-qa][-port][-i][-n][-d][-sn][-pm-ip-addrs][-um-ip-addrs][-pm-count][-um-count]
   -h  Help
   -c  Config File to use to extract configuration data, default is Columnstore.xml.rpmsave
   -u  Upgrade, Install using the Config File from -c, default to Columnstore.xml.rpmsave
	    If ssh-keys aren't setup, you should provide passwords as command line arguments
   -p  Unix Password, used with no-prompting option
   -qs Quick Install - Single Server
   -qm Quick Install - Multi Server
   -port MariaDB ColumnStore Port Address
   -i Non-root Install directory, Only use for non-root installs
   -n Non-distributed install, meaning postConfigure will not install packages on remote nodes
   -d Distributed install, meaning postConfigure will install packages on remote nodes
   -sn System Name
   -pm-ip-addrs Performance Module IP Addresses xxx.xxx.xxx.xxx,xxx.xxx.xxx.xxx
   -um-ip-addrs User Module IP Addresses xxx.xxx.xxx.xxx,xxx.xxx.xxx.xxx
```