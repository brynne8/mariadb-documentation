# Installing MariaDB ColumnStore from the Development Buildbot Package Repositories

NOTE: Document for internal use only

# Introduction

Installing Latest MariaDB ColumnStore developer built packages can be done from the yum, apt-get, and zypper Repositories based on the OS. These are the latest builds from the develop-1.1 branch for the 1.1.x build.

MariaDB ColumnStore packages which consist of:

- MariaDB ColumnStore Platform
- MariaDB ColumnStore API (Bulk Write SDK)
- MariaDB ColumnStore Data-Adapters
- MariaDB ColumnStore Tools

You can install MariaDB ColumnStore Packages from the Repositories for Single-Server installs and for Multi-Server installs utilizing the Non-Distributed feature where the packages are installed on all the servers in the cluster before the configuration and setup is performed.

# MariaDB ColumnStore Packages

Before install, make sure you go through the preparation guide to complete the system setup before installs the packages

[https://mariadb.com/kb/en/library/preparing-for-columnstore-installation-11x/](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-getting-started/preparing-and-installing-mariadb-columnstore-11x/preparing-for-columnstore-installation-11x/)

## CentOS 6

### Using wget to retrieve packages

```sql
wget http://18.232.251.159/repos/latest/mariadb-columnstore/centos/x86_64/6/mariadb-columnstore-1.1.4-1-centos6.x86_64.bin.tar.gz
wget http://18.232.251.159/repos/latest/mariadb-columnstore/centos/x86_64/6/mariadb-columnstore-1.1.4-1-centos6.x86_64.rpm.tar.gz
```

### Adding the MariaDB ColumnStore YUM repository

Add new file with repository information, /etc/yum.repos.d/mariadb-columnstore.repo

```sql
[mariadb-columnstore]
name=MariaDB ColumnStore
baseurl=http://18.232.251.159/repos/latest/mariadb-columnstore/yum/centos/6/x86_64/
gpgkey=http://18.232.251.159/repos/RPM-GPG-KEY-MariaDB-ColumnStore
gpgcheck=1
```

### Installing MariaDB ColumnStore

With the repo file in place you can now install MariaDB ColumnStore like so:

```sql
sudo yum --enablerepo=mariadb-columnstore clean metadata
sudo yum groups mark remove "MariaDB ColumnStore"
sudo yum groupinstall "MariaDB ColumnStore"
```

You can also install this way:

```sql
sudo yum --enablerepo=mariadb-columnstore clean metadata
sudo yum install mariadb-columnstore*
```

Install binary package:

```sql
wget http://18.232.251.159/repos/latest/mariadb-columnstore/centos/x86_64/6/mariadb-columnstore-1.1.4-1-centos6.x86_64.bin.tar.gz
```

## CentOS 7

### Using wget to retrieve packages

```sql
wget http://18.232.251.159/repos/latest/mariadb-columnstore/centos/x86_64/7/mariadb-columnstore-1.1.4-1-centos7.x86_64.bin.tar.gz
wget http://18.232.251.159/repos/latest/mariadb-columnstore/centos/x86_64/7/mariadb-columnstore-1.1.4-1-centos7.x86_64.rpm.tar.gz
```

### Adding the MariaDB ColumnStore YUM repository

Add new file with repository information, /etc/yum.repos.d/mariadb-columnstore.repo

```sql
[mariadb-columnstore]
name=MariaDB ColumnStore
baseurl=http://18.232.251.159/repos/latest/mariadb-columnstore/yum/centos/7/x86_64/
gpgkey=http://18.232.251.159/repos/RPM-GPG-KEY-MariaDB-ColumnStore
gpgcheck=1
```

### Installing MariaDB ColumnStore

With the repo file in place you can now install MariaDB ColumnStore like so:

```sql
sudo yum --enablerepo=mariadb-columnstore clean metadata
sudo yum groups mark remove "MariaDB ColumnStore"
sudo yum groupinstall "MariaDB ColumnStore"
```

You can also install this way:

```sql
sudo yum --enablerepo=mariadb-columnstore clean metadata
sudo yum install mariadb-columnstore*
```

Install binary package:

```sql
wget http://18.232.251.159/repos/latest/mariadb-columnstore/centos/x86_64/7/mariadb-columnstore-1.1.4-1-centos7.x86_64.bin.tar.gz
```

## SUSE 12

### Using wget to retrieve packages

```sql
wget http://18.232.251.159/repos/latest/mariadb-columnstore/suse/x86_64/12//mariadb-columnstore-1.1.4-1-sles12.x86_64.bin.tar.gz
wget http://18.232.251.159/repos/latest/mariadb-columnstore/suse/x86_64/12//mariadb-columnstore-1.1.4-1-sles12.x86_64.rpm.tar.gz
```

### Adding the MariaDB ColumnStore ZYPPER repository

Add new file with repository information, /etc/zypp/repos.d/mariadb-columnstore.repo

```sql
[mariadb-columnstore]
name=mariadb-columnstore
enabled=1
autorefresh=0
baseurl=http://18.232.251.159/repos/latest/mariadb-columnstore/yum/sles/12/x86_64
type=rpm-md
```

Download the MariaDB ColumnStore key

```sql
rpm --import http://18.232.251.159/repos/RPM-GPG-KEY-MariaDB-ColumnStore
```

### Installing MariaDB ColumnStore

With the repo file in place you can now install MariaDB ColumnStore like so:

```sql
sudo zypper refresh
sudo zypper install mariadb-columnstore*
```

Install binary package:

```sql
wget http://18.232.251.159/repos/latest/mariadb-columnstore/suse/x86_64/12/mariadb-columnstore-1.1.4-1-sles12.x86_64.bin.tar.gz
```

## Debian 8

### Using wget to retrieve packages

```sql
wget http://18.232.251.159/repos/latest/mariadb-columnstore/debian/dists/jessie/main/binary_amd64/mariadb-columnstore-1.1.4-1-jessie.amd64.bin.tar.gz
wget http://18.232.251.159/repos/latest/mariadb-columnstore/debian/dists/jessie/main/binary_amd64/mariadb-columnstore-1.1.4-1-jessie.amd64.deb.tar.gz
```

### Adding the MariaDB ColumnStore APT-GET repository

Install  package:

```sql
sudo apt-get install apt-transport-https
```

Add new file with repository information, /etc/apt/sources.list.d/mariab-columnstore.list

```sql
deb http://18.232.251.159/repos/latest/mariadb-columnstore/repo/debian8 jessie main
```

Download the MariaDB ColumnStore key

```sql
wget -qO - http://18.232.251.159/repos/MariaDB-ColumnStore.gpg.key | sudo apt-key add -
```

### Installing MariaDB ColumnStore

With the repo file in place you can now install MariaDB ColumnStore like so:

```sql
sudo apt-get update
sudo apt-get install mariadb-columnstore*
```

Install binary package:

```sql
wget http://18.232.251.159/repos/latest/mariadb-columnstore/debian/dists/jessie/main/binary_adm64/mariadb-columnstore-1.1.4-1-jessie.amd64.bin.tar.gz
```

## Debian 9

### Using wget to retrieve packages

```sql
wget http://18.232.251.159/repos/latest/mariadb-columnstore/debian/dists/stretch/main/binary_amd64/mariadb-columnstore-1.1.4-1-stretch.amd64.bin.tar.gz
wget http://18.232.251.159/repos/latest/mariadb-columnstore/debian/dists/stretch/main/binary_amd64/mariadb-columnstore-1.1.4-1-stretch.amd64.deb.tar.gz
```

### Adding the MariaDB ColumnStore APT-GET repository

Install  package:

```sql
sudo apt-get install apt-transport-https
```

Add new file with repository information, /etc/apt/sources.list.d/mariab-columnstore.list

```sql
deb http://18.232.251.159/repos/latest/mariadb-columnstore/repo/debian9 stretch main
```

Download the MariaDB ColumnStore key

```sql
wget -qO - http://18.232.251.159/repos/MariaDB-ColumnStore.gpg.key | sudo apt-key add -
```

### Installing MariaDB ColumnStore

With the repo file in place you can now install MariaDB ColumnStore like so:

```sql
sudo apt-get update
sudo apt-get install mariadb-columnstore*
```

Install binary package:

```sql
wget http://18.232.251.159/repos/latest/mariadb-columnstore/debian/dists/stretch/main/binary_adm64/mariadb-columnstore-1.1.4-1-stretch.amd64.bin.tar.gz
```

## Ubuntu 16

### Using wget to retrieve packages

```sql
wget http://18.232.251.159/repos/latest/mariadb-columnstore/ubuntu/dists/xenial/main/binary_amd64/mariadb-columnstore-1.1.4-1-xenial.amd64.bin.tar.gz
wget http://18.232.251.159/repos/latest/mariadb-columnstore/ubuntu/dists/xenial/main/binary_amd64/mariadb-columnstore-1.1.4-1-xenial.amd64.deb.tar.gz
```

### Adding the MariaDB ColumnStore APT-GET repository

Add new file with repository information, /etc/apt/sources.list.d/mariab-columnstore.list

```sql
deb http://18.232.251.159/repos/latest/mariadb-columnstore/repo/ubuntu16 xenial main
```

Download the MariaDB ColumnStore key

```sql
wget -qO - http://18.232.251.159/repos/MariaDB-ColumnStore.gpg.key | sudo apt-key add -
```

### Installing MariaDB ColumnStore

With the repo file in place you can now install MariaDB ColumnStore like so:

```sql
sudo apt-get update
sudo apt-get install mariadb-columnstore*
```

## After installation

After the installation completes, follow the configuration and startup steps in the Single-Node and Multi-Node Installation guides using the [Non-Distributed Install feature](https://mariadb.com/kb/en/library/installing-and-configuring-a-multi-server-columnstore-system-11x/#non-distributed-install).

# MariaDB ColumnStore API (Bulk Write SDK) Package

Additional information on the ColumnStore API (Bulk Write SDK):

[https://mariadb.com/kb/en/library/columnstore-bulk-write-sdk/](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-data-ingestion/columnstore-bulk-write-sdk/)

## CentOS 7

### Using wget to retrieve packages

```sql
wget http://18.232.251.159/repos/latest/mariadb-columnstore-api/yum/centos/7/x86_64/mariadb-columnstore-api-1.1.4-1-x86_64-centos7.rpm
```

### Adding the MariaDB ColumnStore API YUM repository

Add new file with repository information, /etc/yum.repos.d/mariadb-columnstore-api.repo

```sql
[mariadb-columnstore-api]
name=MariaDB ColumnStore API
baseurl=http://18.232.251.159/repos/latest/mariadb-columnstore-api/yum/centos/7/x86_64/
gpgkey=http://18.232.251.159/repos/RPM-GPG-KEY-MariaDB-ColumnStore
gpgcheck=1
```

### Installing MariaDB ColumnStore API

With the repo file in place you can now install MariaDB ColumnStore API like so:

```sql
sudo yum --enablerepo=mariadb-columnstore-api clean metadata
sudo yum install mariadb-columnstore-api
```

## Debian 8

### Using wget to retrieve packages

```sql
wget http://18.232.251.159/repos/latest/mariadb-columnstore-api/repo/debian8/pool/main/m/mariadb-columnstore-api/mariadb-columnstore-api_1.1.4_amd64.deb
```

### Adding the MariaDB ColumnStore API APT-GET repository

Install  package:

```sql
sudo apt-get install apt-transport-https
```

Add new file with repository information, /etc/apt/sources.list.d/mariab-columnstore-api.list

```sql
deb http://18.232.251.159/repos/latest/mariadb-columnstore-api/repo/debian8 jessie main```

&lt;&lt;/code&gt;&gt;

Download the MariaDB ColumnStore key

```sql
wget -qO - http://18.232.251.159/repos/MariaDB-ColumnStore.gpg.key | sudo apt-key add -
```

### Installing MariaDB ColumnStore API

With the repo file in place you can now install MariaDB ColumnStore API like so:

```sql
sudo apt-get update
sudo apt-get install mariadb-columnstore-api
```

## Debian 9

### Using wget to retrieve packages

```sql
wget http://18.232.251.159/repos/latest/mariadb-columnstore-api/repo/debian9/pool/main/m/mariadb-columnstore-api/mariadb-columnstore-api_1.1.4_amd64.deb
```

### Adding the MariaDB ColumnStore API APT-GET repository

Install  package:

```sql
sudo apt-get install apt-transport-https
```

Add new file with repository information, /etc/apt/sources.list.d/mariab-columnstore-api.list

```sql
deb http://18.232.251.159/repos/latest/mariadb-columnstore-api/repo/debian9 stretch main
```

Download the MariaDB ColumnStore key

```sql
wget -qO - http://18.232.251.159/repos/MariaDB-ColumnStore.gpg.key | sudo apt-key add -
```

### Installing MariaDB ColumnStore API

With the repo file in place you can now install MariaDB ColumnStore API like so:

```sql
sudo apt-get update
sudo apt-get install mariadb-columnstore-api
```

## Ubuntu 16

### Using wget to retrieve packages

```sql
wget http://18.232.251.159/repos/latest/mariadb-columnstore-api/repo/ubuntu16/pool/main/m/mariadb-columnstore-api/mariadb-columnstore-api_1.1.4_amd64.deb
```

### Adding the MariaDB ColumnStore API APT-GET repository

Add new file with repository information, /etc/apt/sources.list.d/mariab-columnstore-api.list

```sql
deb http://18.232.251.159/repos/latest/mariadb-columnstore-api/repo/ubuntu16 xenial main
```

Download the MariaDB ColumnStore key

```sql
wget -qO - http://18.232.251.159/repos/MariaDB-ColumnStore.gpg.key | sudo apt-key add -
```

### Installing MariaDB ColumnStore API

With the repo file in place you can now install MariaDB ColumnStore API like so:

```sql
sudo apt-get update
sudo apt-get install mariadb-columnstore-api
```

# MariaDB ColumnStore Tools Package

Additional information on the Backup and Restore Tool:

[https://mariadb.com/kb/en/library/backup-and-restore-for-mariadb-columnstore-110-onwards/](https://mariadb.com/kb/en/library/backup-and-restore-for-mariadb-columnstore-110-onwards/)

## CentOS 6

### Adding the MariaDB ColumnStore Tools YUM repository

Add new file with repository information, /etc/yum.repos.d/mariadb-columnstore-tools.repo

```sql
[mariadb-columnstore-tools]
name=MariaDB ColumnStore Tools
baseurl=http://18.232.251.159/repos/latest/mariadb-columnstore-tools/yum/centos/6/x86_64/
gpgkey=http://18.232.251.159/repos/RPM-GPG-KEY-MariaDB-ColumnStore
gpgcheck=1
```

### Installing MariaDB ColumnStore Tools

With the repo file in place you can now install MariaDB ColumnStore Tools like so:

```sql
sudo yum --enablerepo=mariadb-columnstore-tools clean metadata
sudo yum install mariadb-columnstore-tools
```

## CentOS 7

### Adding the MariaDB ColumnStore Tools YUM repository

Add new file with repository information, /etc/yum.repos.d/mariadb-columnstore-tools.repo

```sql
[mariadb-columnstore-tools]
name=MariaDB ColumnStore Tools
baseurl=http://18.232.251.159/repos/latest/mariadb-columnstore-tools/yum/centos/7/x86_64/
gpgkey=http://18.232.251.159/repos/RPM-GPG-KEY-MariaDB-ColumnStore
gpgcheck=1
```

### Installing MariaDB ColumnStore Tools

With the repo file in place you can now install MariaDB ColumnStore Tools like so:

```sql
sudo yum --enablerepo=mariadb-columnstore-tools clean metadata
sudo yum install mariadb-columnstore-tools
```

## SUSE 12

### Adding the MariaDB ColumnStore Tools ZYPPER repository

Add new file with repository information, /etc/zypp/repos.d/mariadb-columnstore-tools.repo

```sql
[mariadb-columnstore-tools]
name=mariadb-columnstore-tools
enabled=1
autorefresh=0
baseurl=http://18.232.251.159/repos/latest/mariadb-columnstore-tools/yum/sles/12/x86_64
type=rpm-md
```

Download the MariaDB ColumnStore key

```sql
rpm --import http://18.232.251.159/repos/RPM-GPG-KEY-MariaDB-ColumnStore
```

### Installing MariaDB ColumnStore

With the repo file in place you can now install MariaDB ColumnStore like so:

```sql
sudo zypper refresh
sudo zypper install mariadb-columnstore-tools
```

## Debian 8

### Adding the MariaDB ColumnStore Tools APT-GET repository

Install  package:

```sql
sudo apt-get install apt-transport-https
```

Add new file with repository information, /etc/apt/sources.list.d/mariab-columnstore-tools.list

```sql
deb http://18.232.251.159/repos/latest/mariadb-columnstore-tools/repo/debian8 jessie main```

&lt;&lt;/code&gt;&gt;

Download the MariaDB ColumnStore key

```sql
wget -qO - http://18.232.251.159/repos/MariaDB-ColumnStore.gpg.key | sudo apt-key add -
```

### Installing MariaDB ColumnStore Tools

With the repo file in place you can now install MariaDB ColumnStore Tools like so:

```sql
sudo apt-get update
sudo apt-get install mariadb-columnstore-tools
```

## Debian 9

### Adding the MariaDB ColumnStore Tools APT-GET repository

Install  package:

```sql
sudo apt-get install apt-transport-https
```

Add new file with repository information, /etc/apt/sources.list.d/mariab-columnstore-tools.list

```sql
deb http://18.232.251.159/repos/latest/mariadb-columnstore-tools/repo/debian9 stretch main
```

Download the MariaDB ColumnStore key

```sql
wget -qO - http://18.232.251.159/repos/MariaDB-ColumnStore.gpg.key | sudo apt-key add -
```

### Installing MariaDB ColumnStore Tools

With the repo file in place you can now install MariaDB ColumnStore Tools like so:

```sql
sudo apt-get update
sudo apt-get install mariadb-columnstore-tools
```

## Ubuntu 16

### Adding the MariaDB ColumnStore Tools APT-GET repository

Add new file with repository information, /etc/apt/sources.list.d/mariab-columnstore-tools.list

```sql
deb http://18.232.251.159/repos/latest/mariadb-columnstore-tools/repo/ubuntu16 xenial main
```

Download the MariaDB ColumnStore key

```sql
wget -qO - http://18.232.251.159/repos/MariaDB-ColumnStore.gpg.key | sudo apt-key add -
```

### Installing MariaDB ColumnStore Tools

With the repo file in place you can now install MariaDB ColumnStore Tools like so:

```sql
sudo apt-get update
sudo apt-get install mariadb-columnstore-tools
```

# MariaDB ColumnStore Data Adapters Packages

Additional information on the Data Adapters:

[https://mariadb.com/kb/en/library/columnstore-streaming-data-adapters/](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-data-ingestion/columnstore-streaming-data-adapters/)

NOTE: MaxScale CDC Connector requires the MariaDB MaxScale package be installed first.

The MariaDB ColumnStore Data Adapters Packages are:

- Kafka Adapters
- Maxscale CDC Adapters

Kafka install package dependencies:

- N/A

Maxscale CDC install package dependencies::

- MaxScale CDC Connector
- MariaDB ColumnStore API
- MariaDB MaxScale

## CentOS 7

### Using wget to retrieve packages

```sql
wget http://18.232.251.159/repos/latest/mariadb-columnstore-data-adapters/yum/centos/7/x86_64/mariadb-columnstore-kafka-adapters-1.1.4-1-x86_64-centos7.rpm
wget http://18.232.251.159/repos/latest/mariadb-columnstore-data-adapters/yum/centos/7/x86_64/mariadb-columnstore-maxscale-cdc-adapters-1.1.4-1-x86_64-centos7.rpm
```

### Adding the MariaDB ColumnStore Data Adapters YUM repository

Add new file with repository information, /etc/yum.repos.d/mariadb-columnstore-data-adapters.repo

```sql
[mariadb-columnstore-data-adapters]
name=MariaDB ColumnStore Data Adapters
baseurl=http://18.232.251.159/repos/latest/mariadb-columnstore-data-adapters/yum/centos/7/x86_64/
gpgkey=http://18.232.251.159/repos/RPM-GPG-KEY-MariaDB-ColumnStore
gpgcheck=1
```

### Installing MariaDB ColumnStore Data Adapters

With the repo file in place you can now install MariaDB ColumnStore Maxscale CDC Data Adapters like so:

```sql
sudo yum --enablerepo=mariadb-columnstore-data-adapters clean metadata
sudo yum install mariadb-columnstore-maxscale-cdc-adapters
```

With the repo file in place you can now install MariaDB ColumnStore Kafka Data Adapters like so:

```sql
sudo yum --enablerepo=mariadb-columnstore-data-adapters clean metadata
sudo yum install mariadb-columnstore-kafka-adapters
```

## Debian 8

### Adding the MariaDB ColumnStore Data Adapters APT-GET repository

### Using wget to retrieve packages

```sql
wget http://18.232.251.159/repos/latest/mariadb-columnstore-data-adapters/repo/debian8/pool/main/m/mariadb-columnstore-kafka-adapters/mariadb-columnstore-kafka-adapters_1.1.4_amd64.deb
wget http://18.232.251.159/repos/latest/mariadb-columnstore-data-adapters/repo/debian8/pool/main/m/mariadb-columnstore-maxscale-cdc-adapters/mariadb-columnstore-maxscale-cdc-adapters_1.1.4_amd64.deb
```

Install  package:

```sql
sudo apt-get install apt-transport-https
```

Add new file with repository information, /etc/apt/sources.list.d/mariab-columnstore-data-adapters.list

```sql
deb http://18.232.251.159/repos/latest/mariadb-columnstore-data-adapters/repo/debian8 jessie main```

&lt;&lt;/code&gt;&gt;

Download the MariaDB ColumnStore key

```sql
wget -qO - http://18.232.251.159/repos/MariaDB-ColumnStore.gpg.key | sudo apt-key add -
```

### Installing MariaDB ColumnStore Data Adapters

With the repo file in place you can now install MariaDB ColumnStore Maxscale CDC Data Adapters like so:

```sql
sudo apt-get update
sudo apt-get install mariadb-columnstore-maxscale-cdc-adapters
```

With the repo file in place you can now install MariaDB ColumnStore Kafka Data Adapters like so:

```sql
sudo apt-get update
sudo apt-get install mariadb-columnstore-kafka-adapters
```

## Debian 9

### Adding the MariaDB ColumnStore Data Adapters APT-GET repository

```sql
wget http://18.232.251.159/repos/latest/mariadb-columnstore-data-adapters/repo/debian9/pool/main/m/mariadb-columnstore-kafka-adapters/mariadb-columnstore-kafka-adapters_1.1.4_amd64.deb
wget http://18.232.251.159/repos/latest/mariadb-columnstore-data-adapters/repo/debian9/pool/main/m/mariadb-columnstore-maxscale-cdc-adapters/mariadb-columnstore-maxscale-cdc-adapters_1.1.4_amd64.deb
```

Install  package:

```sql
sudo apt-get install apt-transport-https
```

Add new file with repository information, /etc/apt/sources.list.d/mariab-columnstore-data-adapters.list

```sql
deb http://18.232.251.159/repos/latest/mariadb-columnstore-data-adapters/repo/debian9 stretch main
```

Download the MariaDB ColumnStore key

```sql
wget -qO - http://18.232.251.159/repos/latest/MariaDB-ColumnStore.gpg.key | sudo apt-key add -
```

### Installing MariaDB ColumnStore Data Adapters

With the repo file in place you can now install MariaDB ColumnStores Maxscale CDC Data Adapters like so:

```sql
sudo apt-get update
sudo apt-get install mariadb-columnstore-maxscale-cdc-adapters
```

With the repo file in place you can now install MariaDB ColumnStores Kafka Data Adapters like so:

```sql
sudo apt-get update
sudo apt-get install mariadb-columnstore-kafka-adapters
```

## Ubuntu 16

```sql
wget http://18.232.251.159/repos/latest/mariadb-columnstore-data-adapters/repo/ubuntu16/pool/main/m/mariadb-columnstore-kafka-adapters/mariadb-columnstore-kafka-adapters_1.1.4_amd64.deb
wget http://18.232.251.159/repos/latest/mariadb-columnstore-data-adapters/repo/ubuntu16/pool/main/m/mariadb-columnstore-maxscale-cdc-adapters/mariadb-columnstore-maxscale-cdc-adapters_1.1.4_amd64.deb
```

### Adding the MariaDB ColumnStore Data Adapters APT-GET repository

Add new file with repository information, /etc/apt/sources.list.d/mariab-columnstore-data-adapters.list

```sql
deb http://18.232.251.159/repos/latest/mariadb-columnstore-data-adapters/repo/ubuntu16 xenial main
```

Download the MariaDB ColumnStore key

```sql
wget -qO - http://18.232.251.159/repos/MariaDB-ColumnStore.gpg.key | sudo apt-key add -
```

### Installing MariaDB ColumnStore Data Adapters

With the repo file in place you can now install MariaDB ColumnStore Maxscale CDC Data Adapters like so:

```sql
sudo apt-get update
sudo apt-get install mariadb-columnstore-maxscale-cdc-adapters
```

With the repo file in place you can now install MariaDB ColumnStore Kafka Data Adapters like so:

```sql
sudo apt-get update
sudo apt-get install mariadb-columnstore-kafka-adapters
```