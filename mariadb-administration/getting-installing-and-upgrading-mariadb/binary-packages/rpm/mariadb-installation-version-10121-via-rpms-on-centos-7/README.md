# MariaDB Installation (Version 10.1.21) via RPMs on CentOS 7

Here are the detailed steps for installing MariaDB (version 10.1.21) via RPMs on CentOS 7.

The RPM's needed for the installation are all available on the MariaDB website and are given below:

- jemalloc-3.6.0-1.el7.x86_64.rpm
- MariaDB-10.1.21-centos7-x86_64-client.rpm
- MariaDB-10.1.21-centos7-x86_64-compat.rpm
- galera-25.3.19-1.rhel7.el7.centos.x86_64.rpm
- jemalloc-devel-3.6.0-1.el7.x86_64.rpm
- MariaDB-10.1.21-centos7-x86_64-common.rpm
- MariaDB-10.1.21-centos7-x86_64-server.rpm

Step by step installation:

- 1) First install all of the dependencies needed. Its easy to do this via YUM packages:
yum install rsync nmap lsof perl-DBI nc
- 2) rpm -ivh jemalloc-3.6.0-1.el7.x86_64.rpm
- 3) rpm -ivh jemalloc-devel-3.6.0-1.el7.x86_64.rpm
- 4) rpm -ivh MariaDB-10.1.21-centos7-x86_64-common.rpm MariaDB-10.1.21-centos7-x86_64-compat.rpm MariaDB-10.1.21-centos7-x86_64-client.rpm galera-25.3.19-1.rhel7.el7.centos.x86_64.rpm MariaDB-10.1.21-centos7-x86_64-server.rpm

While installing MariaDB-10.1.21-centos7-x86_64-common.rpm there might be a conflict with older MariaDB packages. we need to remove them and install the original rpm again.

Here is the error message for dependencies:

```sql
# rpm -ivh MariaDB-10.1.21-centos7-x86_64-common.rpm 
warning: MariaDB-10.1.21-centos7-x86_64-common.rpm: Header V4 DSA/SHA1 Signature, key ID 1bb943db: NOKEY
error: Failed dependencies:
	mariadb-libs < 1:10.1.21-1.el7.centos conflicts with MariaDB-common-10.1.21-1.el7.centos.x86_64
```

Solution:
search for this package:

```sql
# rpm -qa | grep mariadb-libs
mariadb-libs-5.5.52-1.el7.x86_64
```

Remove this package:

```sql
# rpm -ev --nodeps mariadb-libs-5.5.52-1.el7.x86_64
Preparing packages...
mariadb-libs-1:5.5.52-1.el7.x86_64
```

While installing the Galera package there might be a conflict in installation for a dependency package.  
Here is the error message:

```sql
[root@centos-2 /]# rpm -ivh galera-25.3.19-1.rhel7.el7.centos.x86_64.rpm 
error: Failed dependencies:
	libboost_program_options.so.1.53.0()(64bit) is needed by galera-25.3.19-1.rhel7.el7.centos.x86_64

The dependencies for Galera package is: libboost_program_options.so.1.53.0
```

Solution:

```sql
yum install boost-devel.x86_64
```

Another warning message while installing Galera package is as shown below:

```sql
warning: galera-25.3.19-1.rhel7.el7.centos.x86_64.rpm: Header V4 DSA/SHA1 Signature, key ID 1bb943db: NOKEY 
```

The solution for this is to import the key:

```sql
#rpm --import http://yum.mariadb.org/RPM-GPG-KEY-MariaDB
```

After step 4, the installation will be completed. The last step will be to run [mysql_secure_installation](/clients-utilities/mysql_secure_installation/) to secure the production server by dis allowing remote login for root, creating root password and  removing the test database.

- 5) mysql_secure_installation