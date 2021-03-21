# Preparing for ColumnStore Installation - 1.1.X

### Prerequisite

With the 1.1.X  version of MariaDB ColumnStore, there should be no versions of MariaDB Server or MySQL pre-installed on the OS before a MariaDB ColumnStore binary or RPM is installed on the system. If you have an installation of MariaDB server, uninstall it before proceeding.

#### Configuration preparation

Before installing MariaDB ColumnStore, there is some preparation necessary. You will need to determine the following, refer the MariaDB ColumnStore Architecture Document for additional information.

- How many User Modules (UMs) will your system need?

- How many Performance Modules (PMs) will your system need?

- How much disk space will your system need?

- Would have passwordless ssh access between PMs?

- Do you want to run the install as root or noon-root?

##### OS information

MariaDB ColumnStore is certified to run on:

RHEL/CentOS v6, v7

Ubuntu 16.04 LTS

Debian v8, v9

SUSE 12

but it should run on any recent Linux system.

Make sure the same OS is installed on all the servers for a multi-node system.

Make sure the locale setting on all servers are all the same.

To set locale to en_US and UTf-8, run:

```sql
# localedef -i en_US -f UTF-8 en_US.UTF-8
```

##### System administration information

Information your system administrator must provide you before you start installing MariaDB ColumnStore:

- The hostnames of each interface on each node (optional).

- The IP address of each interface on each node.

- The root/non-root password for the nodes (all nodes must have the same root/non-root password or root/non-root ssh keys must be set up between servers). MariaDB ColumnStore can be installed as root or a non-root user.

Example for 3 PM, 1UM system, these are the steps required to configure PM-1 for passwordless ssh. The equivalent steps must be repeated on every PM in the system.
The equivalent steps must be repeated on every UM in the system if the MariaDB ColumnStore Data Replication feature is enabled during the install process.

```sql
[root@pm- 1 ~] $ ssh-keygen
[root@pm- 1 ~] $ ssh-copy-id -i ~/.ssh/id_rsa.pub pm-1
[root@pm- 1 ~] $ ssh-copy-id -i ~/.ssh/id_rsa.pub pm-2
[root@pm- 1 ~] $ ssh-copy-id -i ~/.ssh/id_rsa.pub pm-3
[root@pm- 1 ~] $ ssh-copy-id -i ~/.ssh/id_rsa.pub um-1
```

If utilizing ssh-keys and will be utilizing the MariaDB ColumnStore Data Redundancy feature, make sure you setup an ssh-key on the PM1 node to itself. This is required for Data Redundancy feature.

If utilizing ssh-keys and will be utilizing the MariaDB ColumnStore Data Replication feature, make sure you setup an ssh-key between all UM nodes. And if you enabled the Local Query Feature, will also need between all UM nodes and all PM nodes.

##### Network configuration

MariaDB ColumnStore is quite flexible regarding networking. 
Some options are as follows:

- The interconnect between UMs and PMs can be one or more private VLANs. 
In this case MariaDB ColumnStore will automatically trunk the individual LANs together to provide greater effective bandwidth between the UMs and PMs.

- The PMs do not require a public LAN access as they only need to communicate with the UMs.

- The UMs most likely require at least one public interface to access the MySQL server front end from the site LAN. This interface can be a separate physical or logical connection from the PM interconnect.

- You can use whatever security your site requires on the public access to the MySQL server front end on the UMs. By default it is listening on port 3306.

- MariaDB ColumnStore software only requires a TCP/IP stack to be present to function. You can use any physical layer you desire.

#### MariaDB ColumnStore port usage

The MariaDB ColumnStore daemon utilizes port 3306.

You must reserve the following ports to run the MariaDB ColumnStore
software:
8600 - 8630, 8700, and 8800

#### Storage and Database files (DBRoots)

MariaDB ColumnStore supports being able to configure with different Storage options.

- Internal - Meaning you will be utilize the local root level storage on the server. Also with the Internal storage option, you can use softlinks to map the root level data directory to a SAN storage device. With Internal storage setup, there is no High/Availability Performance Module Failover capabilities. This is one of the limitations to using Internal (local) storage.
- External - Meaning to can map the root level data directory to EXTx SAN storage devices and have all of the Data directories (DBRoot) configured in the Fstab to have shared mounting to all Performance Modules in the system. With External storage setup, there is High/Availability Performance Module Failover capabilities supported. Meaning if a Performance Module was to go down, another Performance Module would automatically mount to the DBRoots that were previously mounted to the downed Performance Module. Then the system functionality would still be working since the system has access to all the of data on the DBRoots.
- Data Redundancy - This feature is support in 1.1 versions and later. MariaDB ColumnStore supports Data Redundancy on both internal root level data directories or if you have them monuted with EXTx SAN devices. For Data Redundancy to be an option, the user is required to install the Third Party GlusterFS software on all Performance Module. This is discussed later the the Package Dependency section. With Data Redundancy storage setup, there is High/Availability Performance Module Failover capabilities supported. Meaning if a Performance Module was to go down, another Performance Module will have a copy of the DBRoots that were on the downed Performance Module. Then the system functionality would still be working since the system has access to all the of data on the DBRoots.

DBRoots are the MariaDB ColumnStore Datafile containers or directories. For example on a root install, they are /usr/local/mariadb/columnstore/data&lt;N&gt; where N is the dbroot number.

IMPORTANT: When using Storage (extX, NFS, etc), setup to have the MariaDB front-end data and the DBRoots back-end data files mounted. Don't setup a mount where the actually MariaDB Columnstore package as a whole is mounted, i.e. /usr/local/mariadb or /usr/local/mariadb/columnstore.

This would included mounts for:

- /usr/local/mariadb/columnstore/mysql/db   # optional for the front-end schemas and non-Columnstore data
- /usr/local/mariadb/columnstore/dataX        # DBroots, Columnstore data

##### Local database files

If you are using local disk to store the database files, the DBRoot directories will be created under the installation directory, for example /usr/local/mariadb/columnstore/data&lt;N&gt; where N is the DBRoot number. 
You should setup to have 1 DBRoot per Performance Module when configuring the system.

Use of soft-links for the Data. If you want to setup an install where the Data is stored out in a separate directory in the case you have limit amount local storage, this can be done.
It is recommended that the softlinks be setup at the Data Directory Levels, like mariadb/columnstore/data and mariadb/columnstore/dataX. With this setup, you can perform upgrades using any of the package types, rpm, debian, or binary.
In the case where you prefer OR have to set a softlink at the top directory, like /usr/local/mariadb, you will need to install using the binary package. If you install using the rpm package and tool, this softlink will be deleted when you perform the upgrade process and the upgrade will fail.

##### SAN mounted database files

If you are using a SAN to store the database files, the following must be taken into account:

- Each of these DBRoots must be a separate, mountable partition/directory

- You might have more than 1 DBRoot assigned to a Performance Module and you can have different number of DBroots per Performance Module, but its recommend to have the same number and same physical size of the storage device on each Performance Module. Here is an example: If you setup 1 Performance Module and you have 2 separate devices that aren't stripped, then you would configure 2 DBRoots to this 1 Performance Module and setup the /etc/fstab with 2 mounts.

- MariaDB ColumnStore will run on most Linux filesystems, but we test most heavily with EXT2. You should have no problems with EXT3 or EXT4, but the journaling in these filesystems can be expensive for a database application. You should carefully evaluate the write characteristics of your chosen filesystem to make sure they meet your specific business needs. In any event, MariaDB ColumnStore writes relatively few, very large (64MB) files. You should consult with your Linux system administrator to see if configuring a larger bytes-per-inode setting than the default is available in your chosen filesystem.

- MariaDB ColumnStore supports High Availability failover in the case where a Performance Module was to go down when you are using SAN storage devices. To setup a system to support this, all of the SAN devices would need to be mountable to all of the Performance Modules. So in a system that had 2 Performance Modules and each one had 1 SAN device. If 1 of the Performance Modules was to go offline, the system would automatically detect this and would remount the SAN device from the downed module to the 1 active Performance Module. And the system would be able to continue to perform while the module remain offline.

- The fstab file must be set up (/etc/fstab). These entries would need to be added to each PM pointing to the all the dbroot(s) being used on all PMs. The 'noauto' option indicates that all dbroots will be associated to every PM but will not be automatically mounted at server startup. The associated dbroots that are assigned to each PM will be specifically mounted to that PM at Columnstore startup.

The following example shows an /etc/fstab setup for 2 dbroots total for all PMs, but they can setup any disk type they want:

```sql
/dev/sda1 /usr/local/mariadb/columnstore/data1 ext2 noatime,nodiratime,noauto 0 0
/dev/sdd1 /usr/local/mariadb/columnstore/data2 ext2 noatime,nodiratime,noauto 0 0
```

#### Performance optimization considerations

There are optimizations that should be made when using MariaDB ColumnStore listed below. As always, please consult with your network administrator for additional optimization considerations for your specific installation needs.

##### GbE NIC settings:

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

NOTE: Make sure there is only 1 setting of net.core.netdev_max_backlog in the /etc/sysctl.conf file.
```

- Cache memory settings:
To optimize Linux to cache directories and inodes the vm.vfs_cache_pressure can be set to a lower value than 100 to attempt to retain caches for inode and directory structures. This will help improve read performance. A value of 10 is suggested. The following commands must all be run as the root user or with sudo.

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

The default setting of 022 in /etc/profile is what is recommended. It it required that it the setting doesn't end with a 7, like 077. Example, on a root install, mysqld that runs as 'mysql' user needs to be able to read the MariaDB ColumnStore configuration file, Columnstore.xml. So a last digit of 7 would prevent this and cause the install to fail.

The current umask can be determined:

```sql
umask
```

A value of 022 can be set in the current session or in /etc/profile as follows:

```sql
umask 022
```

#### Firewall considerations

The MariaDB ColumnStore utilizes these ports 3306, 8600 - 8630, 8700, and 8800. So on multi-node installs, these ports will need to be accessible between the servers. So if there is any firewall software running on the system that could block these ports, either that firewall would need to be disabled as shown below or the ports listed above will need to be configured to allow both input and output on all servers within the firewall software. You will also want to allow these ports to be passed though on any routers that might be connected between the servers.

To disable any local firewalls and SELinux on mutli-node installs

You must be a Root user.

CentOS 6 and systems using iptables

```sql
#service iptables save  (Will save your existing IPTable Rules)
#service iptables stop  (It will disable firewall Temporarly)
```

To Disable it Permanentely:

```sql
#chkconfig iptables off
```

CentOS 7 and systems using systemctl with firewalld installed

```sql
#systemctl status firewalld  
#systemctl stop firewalld  
#systemctl disable firewalld  
```

Ubuntu and systems using ufw

```sql
#service ufw stop  (It will disable firewall Temporary)
```

To Disable it Permanentely:

```sql
#chkconfig ufw off
```

SUSE

```sql
#/sbin/rcSuSEfirewall2 status
#/sbin/rcSuSEfirewall2 stop 
```

To disable SELinux,

```sql
edit file "/etc/selinux/config" and find line;

SELINUX=enforcing

Now replace it with,

SELINUX=disabled
```

### Package dependencies

#### Boost libraries

MariaDB ColumnStore requires that the boost package of 1.53 or newer is installed.

For Centos 7, Ubuntu 16, Debian 8, SUSE 12 and other newer OS's, you can just install the boost packages via yum or apt-get.

```sql
# yum -y install boost
```

or

```sql
# apt-get -y install libboost-all-dev
```

For CentOS 6, you can either download and install the MariaDB Columnstore Centos 6 boost library package or install the boost source of 1.55 and build it to generate the required libraries. That means both the build and the install machines require this.

How to download, go to binaries download page and download "centos6_boost_1_55.tar.gz"

[https://mariadb.com/downloads/columnstore](https://mariadb.com/downloads/columnstore)

Click All Versions - &gt; 1.0.x -&gt; centos -&gt; x86_64

Install package on each server in the cluster

```sql
wget https://downloads.mariadb.com/ColumnStore/1.0.14/centos/x86_64/centos6_boost_1_55.tar.gz
tar xfz centos6_boost_1_55.tar.gz -C /usr/lib
ldconfig
```

Downloading and build the boost libraries:

NOTE: This means that the "Development Tools" group install be done prior to this.

```sql
yum groupinstall "Development Tools"
yum install cmake
```

Here is the procedure to download and build the boost source:

```sql
cd /usr/
wget http://sourceforge.net/projects/boost/files/boost/1.55.0/boost_1_55_0.tar.gz
tar zxvf boost_1_55_0.tar.gz
cd boost_1_55_0
./bootstrap.sh --with-libraries=atomic,date_time,exception,filesystem,iostreams,locale,program_options,regex,signals,system,test,thread,timer,log --prefix=/usr
./b2 install
ldconfig
```

For SUSE 12, you will need to install the boost-devel package, which is part of the SLE-SDK package.

```sql
SUSEConnect -p sle-sdk/12.2/x86_64
zypper install boost-devel
```

#### Other packages

Make sure these packages are installed on the nodes where the MariaDB ColumnStore packages will be installed:

##### Centos 6/7

```sql
# yum -y install expect perl perl-DBI openssl zlib file sudo libaio rsync snappy net-tools numactl-libs nmap jemalloc
```

##### Ubuntu 16/18

```sql
# apt-get -y install tzdata libtcl8.6 expect perl openssl file sudo libdbi-perl libboost-all-dev libreadline-dev rsync libsnappy1v5 snappy net-tools libnuma1 nmap libjemalloc1
```

##### Debian 8

```sql
# apt-get install expect perl openssl file sudo libdbi-perl libboost-all-dev libreadline-dev rsync libsnappy1 net-tools libnuma1 nmap libjemalloc1
```

##### Debian 9

```sql
# apt-get install expect perl openssl file sudo libdbi-perl libboost-all-dev libreadline-dev rsync  net-tools libsnappy1v5 libreadline5 libaio1 libnuma1 nmap libjemalloc1

```

##### SUSE 12

```sql
zypper install expect perl perl-DBI openssl file sudo libaio1 rsync net-tools libsnappy1 libnuma1 nmap jemalloc
```

#### Data Redundancy packages

In 1.1.0 and later, MariaDB ColumnStore supports Data Redundancy by utilize the third party software GlusterFS. So if you wanted to utilize the Data Redundancy feature, you would need to install the the GlusterFS package on all nodes that will be used as Performance Modules.
Requires 3.3.1 version or later.

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
# sudo apt-get install glusterfs-server attr

start / enable service:
# sudo systemctl start glusterfs-server.service 
# sudo systemctl enable glusterfs-server.service 
```

##### Debian 8

```sql
# apt-get -y install glusterfs-server attr

start / enable service:
# update-rc.d glusterfs-server defaults
```

##### Debian 9

```sql
# sudo apt-get install glusterfs-server attr

start / enable service:
# sudo systemctl start glusterfs-server.service 
# sudo systemctl enable glusterfs-server.service 
```

##### SUSE 12

```sql
# zypper install glusterfs

start / enable service:
# chkconfig glusterd on
```

#### System Logging Package

MariaDB ColumnStore utilizes the System Logging applications for generating logs. So one of the below system logging applications should be install on all servers in the ColumnStore system:

- syslog
- rsyslog
- syslog-ng

### Choosing the type of initial download/install

Installing MariaDB ColumnStore with the use of soft-links. If you want to setup an install where the Data is stored out in a separate directory in the case you have limit amount local storage, this can be done.
It is requied that the softlinks be setup at the Data Directory Levels, like mariadb/columnstore/data and mariadb/columnstore/dataX. With this setup, you can perform upgrades using any of the package types, rpm, debian, or binary.
Dont set a softlink at the top directory, like /usr/local/mariadb or /usr/local/mariadb/columnstore.

IMPORTANT: Make sure there are no other version of MariaDB server install. If so, these will need to be uninstalled before installing MariaDB ColumnStore.

#### Where to Install

The default MariaDB ColumnStore installation process supports a Distributed installation functionality for Multi-Node installs. With Distributed,the MariaDB ColumnStore packages only needed to be installed on the Performance Module #1 node. MariaDB ColumnStore would distributed the packages across to the other nodes in the system and will install them automatically.

In the 1.1 version and later, MariaDB ColumnStore supports a Non-Distributed option for Multi-Node installs. With this option, the user is required to install the MariaDB ColumnStore on all the nodes in the system.

### Root user installs

#### Initial download/install of MariaDB ColumnStore Package Repositories

The MariaDB ColumnStore RPM and DEB Packages can be installed using yum/apt-get commands.
Check here for additional Information:

[https://mariadb.com/kb/en/library/installing-mariadb-ax-from-the-package-repositories](https://mariadb.com/kb/en/library/installing-mariadb-ax-from-the-package-repositories)

#### Initial download/install of MariaDB ColumnStore Package with the AX package

The MariaDB ColumnStore RPM, DEB and binary Packages can be installed along with all of the other packages that make up the AX package.
Check here for additional Information:

[https://mariadb.com/kb/en/library/columnstore-getting-started-installing-mariadb-ax-from-the-mariadb-download/](https://mariadb.com/kb/en/library/columnstore-getting-started-installing-mariadb-ax-from-the-mariadb-download/)

#### Initial download/install of MariaDB ColumnStore RPMs

1 Install MariaDB ColumnStore as user root (use 'su -' to establish a login shell if you access the box using another account):

Note: MariaDB ColumnStore installation will install with a single MariaDB userid of
root with no password. You may setup users and permissions for a MariaDB
ColumnStore-Mysql account just as you would in MySQL.

Note: The packages will be installed into /usr/local. This is required for root user installs

<strong> Download the package mariadb-columnstore-release#.x86_64.tar.gz (RHEL5 64-BIT) to the server where you are installing MariaDB ColumnStore and place in the /root directory.
</strong> Unpack the tarball, which will generate multiple RPMs that will reside in the
/root/ directory. 
<br>
<code class="fixed" style="white-space:pre-wrap">tar -zxf mariadb-columnstore-release#.x86_64.tar</code> 
<br>
<strong> Install the RPMs. The MariaDB ColumnStore software will be installed in /usr/local/. <br>
<code class="fixed" style="white-space:pre-wrap">rpm -ivh mariadb-columnstore*release#*.rpm</code>
<br></strong>

#### Initial download/install of MariaDB ColumnStore binary package

The MariaDB ColumnStore binary package can be installed from the MariaDB Downloads page. 
&lt;CLICK&gt; on the "all versions" link and then step down into the OS version to the bottom level directory where you will see 2 packages, one for rpm/deb and the bother for the binary. Select the binary package to download.

Install MariaDB ColumnStore as user root on the server designated as PM1:
Note: You may setup users and permissions for an MariaDB ColumnStore account just as you would in MariaDB.

<strong> For root user installs, MariaDB Columnstore needs to run in /usr/local.
You can either install directly into /usr/local or install elsewhere and then setup a softlink
to /usr/local. Here is an example of setting up a soft-link if you install the binary package in /mnt/mariadb</strong>

```sql
# ln -s /mnt/mariadb /usr/local
```

- Download the package into the /root/ and copy to /usr/local directory to the server where you are installing MariaDB ColumnStore. <br>

```sql
cp /root/mariadb-columnstore-release#.x86_64.bin.tar.gz /usr/local/ mariadb-columnstore-release#.x86_64.bin.tar.gz
```

- Unpack the tarball, which will generate the /usr/local/ directory. <br>
<code class="fixed" style="white-space:pre-wrap">tar -zxvf mariadb-columnstore-release#.x86_64.bin.tar.gz</code>

Run the post-install script: <br>

```sql
/usr/local/mariadb/columnstore/bin/post-install
```

#### Initial download/install of MariaDB ColumnStore DEB package

DEB package installs are not supported in the current version, but there is an Ubuntu 16.04 binary package that you can use to install. Just follow the binary package instructions above

Install MariaDB ColumnStore on a Debian or Ubuntu OS as user root:
Note: You may setup users and permissions for an MariaDB ColumnStore
account just as you would in MariaDB.

1 Download the package mariadb-columnstore-release#.amd64.deb.tar.gz <br>
(DEB 64- BIT) into the /root directory of the server where you are installing MariaDB ColumnStore. <br>
2 Unpack the tarball, which will generate DEBs. <br>
<code class="fixed" style="white-space:pre-wrap"> tar -zxf  mariadb-columnstore-release#.amd64.deb.tar.gz</code> <br>
3 Install the MariaDB ColumnStore DEBs. The MariaDB ColumnStore software will be installed in /usr/
local/. <br>
<code class="fixed" style="white-space:pre-wrap"> dpkg -i mariadb-columnstore*release#*.deb</code>

### Non-root user installs

MariaDB Columnstore can be installed to run as a non-root user using the binary tar file installation. These procedures will also allow you to change the installation from the default install directory into a user-specified directory. These procedures will need to be run on all the MariaDB ColumnStore Servers.

Non-root user, since the MariaDB Server runs under the user mysql, you can also install the MariaDB ColumnStore package and run as the mysql user. So below where it shows 'guest' as the user, you can substitute 'mysql'.

For the purpose of these instructions, the following assumptions are:

- Non-root user "guest" is used in this example
- Installation directory is /home/guest/mariadb/columnstore

Tasks involved:

- Create the non-root user and group of the same name (by root user)
- Update sudo configuration (by root user)
- Set the user file limits (by root user)
- Modify fstab if using SAN Mounted files (by root user)
- Uninstall existing MariaDB Columnstore installation if needed (by root user)
- Update permissions on certain directories that MariaDB Columnstore writes (by root user)
- MariaDB Columnstore Installation (by non-root user)
- Enable MariaDB Columnstore to start automatically at boot time

#### Creation of the non-root user (by root user)

Before beginning the binary tar file installation you will need your system administrator to set up accounts
for you on every MariaDB Columnstore node. The account name must be the same on every node. The password used must be the same on every node. If you subsequently change the password on one node, you must change it on every node. The user-id must be the same on every node as well. In the examples below we will use the account name 'guest' and the password 'mariadb'. Additionally, every node must have a basic
Linux server package setup and additionally have expect (and all its dependencies) installed.

- create new user

Group ID is an example, can be different than 1000, but needs to be the same on all servers in the cluster

<code class="fixed" style="white-space:pre-wrap">adduser guest -u 1000</code>

- create group

```sql
addgroup guest
moduser -g guest guest
```

The value for user-id must be the same for all nodes.

- Assign password to newly created user

<code class="fixed" style="white-space:pre-wrap">passwd guest</code>

- Log in as user guest

<code class="fixed" style="white-space:pre-wrap">su - guest</code>

- Choose an installation directory in which the non-root user has full read-write access. The
installation directory must be the same on every node. In the examples below we will use the path
'/home/guest/mariadb/columnstore'. <br>
On each host, the following will be in a /etc/profile.d/columnstore.sh <br>
This file is setup during install.

```sql
export COLUMNSTORE_INSTALL_DIR=$HOME/mariadb/columnstore
export PATH=$COLUMNSTORE_INSTALL_DIR/bin:$COLUMNSTORE_INSTALL_DIR/mysql/bin:/usr/sbin:$PATH
export LD_LIBRARY_PATH=$COLUMNSTORE_INSTALL_DIR/lib:$COLUMNSTORE_INSTALL_DIR/mysql/lib
```

#### Update sudo configuration (by root user)

The sudo configuration file on each node will need to be modified to add in the non-root user. The
recommended way is to use the Unix command, visudo. The following example will add the ‘guest’
user:
visudo

- Add the following line for the non-root user: <br>

```sql
guest  ALL=(ALL)       NOPASSWD: ALL
```

- Comment out the following line, which will allow the user to login without 'tty':

```sql
#Defaults requiretty
```

#### Set the user file limits (by root user)

ColumnStore needs the open file limit to be increased for the specified user. To do this edit the /etc/security/limits.conf file and make the following additions at the end of the file:

```sql
guest hard nofile 65536
guest soft nofile 65536
```

If you are already logged in as 'guest' you will need to log out and back in again for this change to take effect.

#### Modify fstab if using SAN Mounted Database Files (by root user)

If you are using a SAN to store the database files, an ‘users‘ option will need to be added to the fstab
entries (by the root user). For more information, please see the “SAN Mounted Database Files” section
earlier in this guide.

<br>Example entries: <br>
/dev/sda1 /home/guest/mariadb/columnstore/data1 ext2 noatime,nodiratime,noauto,users 0 0 <br>
/dev/sdd1 /home/mariadb/columnstore/data2 ext2 noatime,nodiratime,noauto,users 0 0 <br>

The disk device being used will need to have its user permissions set to the non-root user name. This is an
example command run as 'root' user setting the user ownership of dbroot /dev/sda1 to non-root user of
'guest': <br>

```sql
mke2fs dbroot (i.e., /dev/sda1) 
mount /dev/sda1 /tmpdir 
chown -R infinidb.infinidb /tmpdir 
umount /tmpdir 
```

#### Uninstall existing MariaDB Columnstore installation, if needed (by root user) <br>

If MariaDB Columnstore has ever before been installed on any of the planned hosts as a root user install, you must have the system administrator verify that no remnants of that installation exist. The non-root installation will not be successful if there are MariaDB Columnstore files owned by root on any of the hosts.

- Verify the MariaDB Columnstore installation directory does not exist: <br>
The /usr/local/mariadb/columnstore directory should not exist at all unless it is your target directory, in
which case it must be completely empty and owned by the non-root user.

- Verify the /etc/fstab entries are correct for the new installation. <br>

- Verify the /etc/default/columnstore directory does not exist.<br>

- Verify the /var/lock/subsys/mysql-Columnstore file does not exist.<br>

- Verify the /tmp/StopColumnstore file does not exist. <br>

There should not be any files or directories owned by root in the /tmp directory

#### Update permissions on certain directories that MariaDB Columnstore writes (by root user)

These directories are writing to by the MariaDB Columnstore applications and the permissions need to be set to allow them to create files. So the permissions of them need to be set to the following by root user.

```sql
chmod 777 /tmp
chmod 777 /dev/shm
```

#### MariaDB Columnstore installation (by non-root user)

You should be familiar with the general MariaDB Columnstore installation instructions in this guide as you will be asked the same questions during installation.

If performing the MariaDB ColumnStore Multi-node Distributed Installation, the binary package only needs to be installed on Performance Module #1 (pm1).  If performing Non-Distributed installs, the binary package is required to install the MariaDB ColumnStore on all the nodes in the system.

- Log in as non-root user ( guest , in our example)
Note: Ensure you are at your home directory before proceeding to the next step

- Now place the MariaDB Columnstore binary tar file in your home directory on the host you will be using as
PM1. Untar the binary distribution package to the /home/guest directory:
tar -xf mariadb-columnstore-release#.x86_64.bin.tar.gz

- Run post installation:

If performing the MariaDB ColumnStore Multi-node Distributed Installation, the post-install only needs to be run on Performance Module #1 (pm1).  If performing Non-Distributed installs, the post-install needs to be run the MariaDB ColumnStore on all the nodes in the system.

```sql
./mariadb/columnstore/bin/post-install --installdir=$HOME/mariadb/columnstore
```

- Run the 2 export command lines that were outputted by the previous post-install command, which would
look like the following. See the “MariaDB Columnstore Configuration” in this guide for more information:

```sql
export COLUMNSTORE_INSTALL_DIR=/home/guest/mariadb/columnstore
export LD_LIBRARY_PATH=/home/guest/mariadb/columnstore/lib:/home/guest/mariadb/columnstore/mysql/lib
/home/guest/mariadb/columnstore/bin/postConfigure -i /home/guest/mariadb/columnstore
```

- Start MariaDB ColumnStore Service for Non-Distributed installs

If performing the MariaDB ColumnStore Multi-node Non-Distributed installs, the 'columnstore' service needs to be started on all the nodes in the system except Performance Module #1 (pm1). postConfigure is run from pm1, which is documented in the next section

Starting the 'columnstore' service:

```sql
./mariadb/columnstore/bin/columnstore start
```

#### Post-installation (by root user)

Optional items to assist in MariaDB Columnstore auto-start and logging:

- To configure MariaDB Columnstore to start automatically at boot time, perform the following steps in each
Columnstore server:

- Add the following to the /etc/rc.local or /etc/rc.d/rc.local (centos7) file:

su - guest -l -c "/home/guest/mariadb/columnstore/bin/columnstore start"

or

sudo runuser -l mariadb-user -c "/home/mariadb-user/mariadb/columnstore/bin/columnstore start"

Note: Make sure the above entry is added to the rc.local file that gets executed at boot time.
Depending on the OS installation, rc.local could be in a different location.

- MariaDB Columnstore will setup and log using your current system logging application in the directory
/var/log/mariadb/columnstore.

### ColumnStore Cluster Test Tool

This tool can be running before doing installation on a single-server or multi-node installs. It will verify the setup of all servers that are going to be used in the Columnstore System.

[https://mariadb.com/kb/en/mariadb/mariadb-columnstore-cluster-test-tool](https://mariadb.com/kb/en/mariadb/mariadb-columnstore-cluster-test-tool)

The next step would be to run the install script postConfigure, check the Single Server Or Multi-Server Install guide.

### ColumnStore Configuration and Installation Tool

MariaDB Columnstore System Configuration and Installation tool, 'postConfigure', will Configure the MariaDB Columnstore System and will perform a Package Installation of all of the Servers within the System that is being configured. It will prompt the user to configuration information like server, storage, MariaDB Server, and system features. It updates the MariaDB Columnstore System Configuration File, Columnstore.xml. At the end, it will start up the ColumnStore system.

NOTE:  When prompted for password, enter the non-user account password OR just hit enter if you
have setup the non-root user with password-less ssh keys on all nodes (Please see the “System
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