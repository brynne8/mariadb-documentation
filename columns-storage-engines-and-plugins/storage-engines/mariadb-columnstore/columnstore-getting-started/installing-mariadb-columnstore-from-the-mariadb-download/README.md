# Installing MariaDB ColumnStore from the MariaDB Download

# Introduction

MariaDB ColumnStore packages can be downloaded from the [MariaDB Downloads](https://mariadb.com/downloads/#mariadb-platform-mariadb_columnstore)

MariaDB ColumnStore packages:

- MariaDB ColumnStore Platform
- MariaDB ColumnStore API (Bulk Write SDK)
- MariaDB ColumnStore Data-Adapters
<ul start="1"><li>MaxScale kafka
</li><li>Kafka Avro
</li><li>Kettle bulk exporter plugin
</li><li>Maxscale CDC
</li></ul>
- MariaDB ColumnStore Tools
- MariaDB Maxscale
- MariaDB Connectors
<ul start="1"><li>MariaDB Maxscale CDC Connector
</li><li>MariaDB ODBC Connector
</li><li>MariaDB Java-client Connector jar file
</li></ul>

NOTE: Centos 6 and Suse 12 doesn't contain all of the above packages. Some features arent supported for those 2 OSs.

You can install MariaDB ColumnStore packages for Single-Server installs and for Multi-Server installs. For Multi-Server systems, the Downloaded package will need to be installed on all servers in the ColumnStore deployment.

Before install, make sure you go through the preparation guide to complete the system setup before installs the packages

[https://mariadb.com/kb/en/library/preparing-for-columnstore-installation-11x/](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-getting-started/preparing-and-installing-mariadb-columnstore-11x/preparing-for-columnstore-installation-11x)

NOTE: the Release is shown as x.x.x-x. replace that will the matching version that is downloaded. Example 1.1.3-1.

# Centos 6

## Centos 6 RPM package

Once the package is downloaded, it can be untarred with the following command:

```sql
tar zxvf mariadb-columnstore-x.x.x-x-centos6.x86_64.rpm.tar.gz
```

This is the contents of the installed package:

- MariaDB-ColumnStore/
<ul start="1"><li>mariadb-columnstore-1.1.3-1-centos6.x86_64.rpm.tar.gz
</li></ul>
- MariaDB-ColumnStore-Tools/
<ul start="1"><li>mariadb-columnstore-tools-1.1.3-1.rpm
</li></ul>
- MariaDB-Connectors/
<ul start="1"><li>mariadb-connector-[ODBC-3](https://jira.mariadb.org/browse/ODBC-3).0.2-ga-rhel6-i686.tar.gz
</li><li>mariadb-java-client-2.2.2.jar
</li></ul>
- MariaDB-MaxScale/
<ul start="1"><li>maxscale-2.2.2-1.centos.6.x86_64.rpm
</li><li>maxscale-cdc-connector-2.2.2-1.centos.6.x86_64.rpm
</li></ul>

## Centos 6 RPM package installation

### MariaDB ColumnStore

Follow the instructions for installing MariaDB Columnstore RPMs

[https://mariadb.com/kb/en/library/preparing-for-columnstore-installation-11x/#choosing-the-type-of-initial-downloadinstall](https://mariadb.com/kb/en/library/preparing-for-columnstore-installation-11x/#choosing-the-type-of-initial-downloadinstall)

### MariaDB ColumnStore Tools

Follow the instructions for installing MariaDB Columnstore Tools

[https://mariadb.com/kb/en/library/backup-and-restore-for-mariadb-columnstore-110-onwards/](https://mariadb.com/kb/en/library/backup-and-restore-for-mariadb-columnstore-110-onwards/)

### MariaDB ColumnStore Connectors

#### ODBC Connector

How to Install the ODBC Connector:

[https://mariadb.com/kb/en/library/about-mariadb-connector-odbc/](https://mariadb.com/kb/en/library/about-mariadb-connector-odbc/)

Additional Information on the ODBC connector:

[https://mariadb.com/kb/en/library/mariadb-connector-odbc/](https://mariadb.com/kb/en/library/mariadb-connector-odbc/)

#### Java Connector

How to Install the Java Connector:

[https://mariadb.com/kb/en/library/getting-started-with-the-java-connector/](https://mariadb.com/kb/en/library/getting-started-with-the-java-connector/)

Additional information on the Java Connector:

[https://mariadb.com/kb/en/library/mariadb-connector-j/](https://mariadb.com/kb/en/library/mariadb-connector-j/)

How to Install on the MariaDB MaxScale:

[https://mariadb.com/kb/en/mariadb-enterprise/mariadb-maxscale-22-installing-mariadb-maxscale-using-a-tarball/](https://mariadb.com/kb/en/mariadb-enterprise/mariadb-maxscale-22-installing-mariadb-maxscale-using-a-tarball/)

## Centos 6 Binary Package

Once the package is downloaded, it can be untarred with the following command:

```sql
tar zxvf mariadb-columnstore-x.x.x-x-centos6.x86_64.bin.tar.gz
```

This is the contents of the installed package:

- MariaDB-ColumnStore/
<ul start="1"><li>mariadb-columnstore-1.1.3-1-centos6.x86_64.bin.tar.gz
</li></ul>
- MariaDB-ColumnStore-Tools/
<ul start="1"><li>mariadb-columnstore-tools-1.1.3-1.bin.tar.gz
</li></ul>
- MariaDB-Connectors/
<ul start="1"><li>mariadb-connector-[ODBC-3](https://jira.mariadb.org/browse/ODBC-3).0.2-ga-rhel6-i686.tar.gz
</li><li>mariadb-java-client-2.2.2.jar
</li></ul>
- MariaDB-MaxScale/
<ul start="1"><li>maxscale-2.2.2-1.centos.6.tar.gz
</li><li>maxscale-cdc-connector-2.2.2-1.centos.6.x86_64.rpm
</li></ul>

## Centos 6 Binary Package installation

### MariaDB ColumnStore

Follow the instructions for installing MariaDB Columnstore binary package

[https://mariadb.com/kb/en/library/preparing-for-columnstore-installation-11x/#choosing-the-type-of-initial-downloadinstall](https://mariadb.com/kb/en/library/preparing-for-columnstore-installation-11x/#choosing-the-type-of-initial-downloadinstall)

### MariaDB ColumnStore Tools

Follow the instructions for installing MariaDB Columnstore Tools

[https://mariadb.com/kb/en/library/backup-and-restore-for-mariadb-columnstore-110-onwards/](https://mariadb.com/kb/en/library/backup-and-restore-for-mariadb-columnstore-110-onwards/)

### MariaDB ColumnStore Connectors

#### ODBC Connector

How to Install the ODBC Connector:

[https://mariadb.com/kb/en/library/about-mariadb-connector-odbc/](https://mariadb.com/kb/en/library/about-mariadb-connector-odbc/)

Additional Information on the ODBC connector:

[https://mariadb.com/kb/en/library/mariadb-connector-odbc/](https://mariadb.com/kb/en/library/mariadb-connector-odbc/)

#### Java Connector

How to Install the Java Connector:

[https://mariadb.com/kb/en/library/getting-started-with-the-java-connector/](https://mariadb.com/kb/en/library/getting-started-with-the-java-connector/)

Additional information on the Java Connector:

[https://mariadb.com/kb/en/library/mariadb-connector-j/](https://mariadb.com/kb/en/library/mariadb-connector-j/)

### MariaDB MaxScale

How to Install on the MariaDB MaxScale:

[https://mariadb.com/kb/en/mariadb-enterprise/mariadb-maxscale-22-installing-mariadb-maxscale-using-a-tarball/](https://mariadb.com/kb/en/mariadb-enterprise/mariadb-maxscale-22-installing-mariadb-maxscale-using-a-tarball/)

# Centos 7

## Centos 7 RPM package

Once the package is downloaded, it can be untarred with the following command:

```sql
tar zxvf mariadb-columnstore-x.x.x-x-centos7.x86_64.rpm.tar.gz
```

This is the contents of the installed package:

- MariaDB-ColumnStore/
<ul start="1"><li>mariadb-columnstore-1.1.3-1-centos7.x86_64.rpm.tar.gz
</li></ul>
- MariaDB-ColumnStore-API/
<ul start="1"><li>mariadb-columnstore-api-1.1.3-1-x86_64-centos7.rpm
</li></ul>
- MariaDB-ColumnStore-Tools/
<ul start="1"><li>mariadb-columnstore-tools-1.1.3-1.rpm
</li></ul>
- MariaDB-Connectors/
<ul start="1"><li>mariadb-connector-[ODBC-3](https://jira.mariadb.org/browse/ODBC-3).0.2-ga-rhel7-i686.tar.gz
</li><li>mariadb-java-client-2.2.2.jar
</li></ul>
- MariaDB-Data-Adapters/
<ul start="1"><li>mariadb-columnstore-kafka-adapters-1.1.3-1-x86_64-centos7.rpm
</li><li>mariadb-columnstore-maxscale-cdc-adapters-1.1.3-1-x86_64-centos7.rpm
</li></ul>
- MariaDB-MaxScale/
<ul start="1"><li>maxscale-2.2.2-1.centos.7.x86_64.rpm
</li><li>maxscale-cdc-connector-2.2.2-1.centos.7.x86_64.rpm
</li></ul>

## Centos 7 RPM package installation

### MariaDB ColumnStore

Follow the instructions for installing MariaDB Columnstore RPMs

[https://mariadb.com/kb/en/library/preparing-for-columnstore-installation-11x/#choosing-the-type-of-initial-downloadinstall](https://mariadb.com/kb/en/library/preparing-for-columnstore-installation-11x/#choosing-the-type-of-initial-downloadinstall)

### MariaDB ColumnStore API (Bulk Write SDK)

Follow the instructions for installing MariaDB Columnstore API RPM

[https://mariadb.com/kb/en/library/columnstore-bulk-write-sdk/](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-data-ingestion/columnstore-bulk-write-sdk)

### MariaDB ColumnStore Tools

Follow the instructions for installing MariaDB Columnstore Tools

[https://mariadb.com/kb/en/library/backup-and-restore-for-mariadb-columnstore-110-onwards/](https://mariadb.com/kb/en/library/backup-and-restore-for-mariadb-columnstore-110-onwards/)

### MariaDB ColumnStore Connectors

#### ODBC Connector

How to Install the ODBC Connector:

[https://mariadb.com/kb/en/library/about-mariadb-connector-odbc/](https://mariadb.com/kb/en/library/about-mariadb-connector-odbc/)

Additional Information on the ODBC connector:

[https://mariadb.com/kb/en/library/mariadb-connector-odbc/](https://mariadb.com/kb/en/library/mariadb-connector-odbc/)

#### Java Connector

How to Install the Java Connector:

[https://mariadb.com/kb/en/library/getting-started-with-the-java-connector/](https://mariadb.com/kb/en/library/getting-started-with-the-java-connector/)

Additional information on the Java Connector:

[https://mariadb.com/kb/en/library/mariadb-connector-j/](https://mariadb.com/kb/en/library/mariadb-connector-j/)

### MariaDB ColumnStore Data Adapters

How to Install on the Data Adapters:

[https://mariadb.com/kb/en/library/columnstore-streaming-data-adapters/](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-data-ingestion/columnstore-streaming-data-adapters)

### MariaDB MaxScale

How to Install on the MariaDB MaxScale:

[https://mariadb.com/kb/en/mariadb-enterprise/mariadb-maxscale-22-installing-mariadb-maxscale-using-a-tarball/](https://mariadb.com/kb/en/mariadb-enterprise/mariadb-maxscale-22-installing-mariadb-maxscale-using-a-tarball/)

## Centos 7 Binary Package

Once the package is downloaded, it can be untarred with the following command:

```sql
tar zxvf mariadb-columnstore-x.x.x-x-centos7.x86_64.bin.tar.gz
```

This is the contents of the installed package:

- MariaDB-ColumnStore/
<ul start="1"><li>mariadb-columnstore-1.1.3-1-centos7.x86_64.bin.tar.gz
</li></ul>
- MariaDB-ColumnStore-API/
<ul start="1"><li>mariadb-columnstore-api-1.1.3-1-x86_64-centos7.rpm
</li></ul>
- MariaDB-ColumnStore-Tools/
<ul start="1"><li>mariadb-columnstore-tools-1.1.3-1.bin.tar.gz
</li></ul>
- MariaDB-Connectors/
<ul start="1"><li>mariadb-connector-[ODBC-3](https://jira.mariadb.org/browse/ODBC-3).0.2-ga-rhel7-i686.tar.gz
</li><li>mariadb-java-client-2.2.2.jar
</li></ul>
- MariaDB-Data-Adapters/
<ul start="1"><li>mariadb-columnstore-kafka-adapters-1.1.3-1-x86_64-centos7.rpm
</li><li>mariadb-columnstore-maxscale-cdc-adapters-1.1.3-1-x86_64-centos7.rpm
</li></ul>
- MariaDB-MaxScale/
<ul start="1"><li>maxscale-2.2.2-1.centos.7.tar.gz
</li><li>maxscale-cdc-connector-2.2.2-1.centos.7.x86_64.rpm
</li></ul>

## Centos 7 Binary Package installation

### MariaDB ColumnStore

Follow the instructions for installing MariaDB Columnstore binary package

[https://mariadb.com/kb/en/library/preparing-for-columnstore-installation-11x/#choosing-the-type-of-initial-downloadinstall](https://mariadb.com/kb/en/library/preparing-for-columnstore-installation-11x/#choosing-the-type-of-initial-downloadinstall)

### MariaDB ColumnStore API (Bulk Write SDK)

Follow the instructions for installing MariaDB Columnstore API RPM

[https://mariadb.com/kb/en/library/columnstore-bulk-write-sdk/](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-data-ingestion/columnstore-bulk-write-sdk)

### MariaDB ColumnStore Tools

Follow the instructions for installing MariaDB Columnstore Tools

[https://mariadb.com/kb/en/library/backup-and-restore-for-mariadb-columnstore-110-onwards/](https://mariadb.com/kb/en/library/backup-and-restore-for-mariadb-columnstore-110-onwards/)

### MariaDB ColumnStore Connectors

#### ODBC Connector

How to Install the ODBC Connector:

[https://mariadb.com/kb/en/library/about-mariadb-connector-odbc/](https://mariadb.com/kb/en/library/about-mariadb-connector-odbc/)

Additional Information on the ODBC connector:

[https://mariadb.com/kb/en/library/mariadb-connector-odbc/](https://mariadb.com/kb/en/library/mariadb-connector-odbc/)

#### Java Connector

How to Install the Java Connector:

[https://mariadb.com/kb/en/library/getting-started-with-the-java-connector/](https://mariadb.com/kb/en/library/getting-started-with-the-java-connector/)

Additional information on the Java Connector:

[https://mariadb.com/kb/en/library/mariadb-connector-j/](https://mariadb.com/kb/en/library/mariadb-connector-j/)

### MariaDB ColumnStore Data Adapters

How to Install on the Data Adapters:

[https://mariadb.com/kb/en/library/columnstore-streaming-data-adapters/](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-data-ingestion/columnstore-streaming-data-adapters)

### MariaDB MaxScale

How to Install on the MariaDB MaxScale:

[https://mariadb.com/kb/en/mariadb-enterprise/mariadb-maxscale-22-installing-mariadb-maxscale-using-a-tarball/](https://mariadb.com/kb/en/mariadb-enterprise/mariadb-maxscale-22-installing-mariadb-maxscale-using-a-tarball/)

# Suse 12

## Suse 12 RPM package

Once the package is downloaded, it can be untarred with the following command:

```sql
tar zxvf mariadb-columnstore-x.x.x-x-sles12.x86_64.rpm.tar.gz
```

This is the contents of the installed package:

- MariaDB-ColumnStore/
<ul start="1"><li>mariadb-columnstore-1.1.3-1-sles12.x86_64.rpm.tar.gz
</li></ul>
- MariaDB-ColumnStore-Tools/
<ul start="1"><li>mariadb-columnstore-tools-1.1.3-1.rpm
</li></ul>
- MariaDB-Connectors/
<ul start="1"><li>mariadb-java-client-2.2.2.jar
</li></ul>
- MariaDB-MaxScale/
<ul start="1"><li>maxscale-2.2.2-1.sles/12.x86_64.rpm
</li><li>maxscale-cdc-connector-2.2.2-1.sles.12.x86_64.rpm
</li></ul>

## Suse 12 RPM package installation

### MariaDB ColumnStore

Follow the instructions for installing MariaDB Columnstore RPMs

[https://mariadb.com/kb/en/library/preparing-for-columnstore-installation-11x/#choosing-the-type-of-initial-downloadinstall](https://mariadb.com/kb/en/library/preparing-for-columnstore-installation-11x/#choosing-the-type-of-initial-downloadinstall)

### MariaDB ColumnStore Tools

Follow the instructions for installing MariaDB Columnstore Tools

[https://mariadb.com/kb/en/library/backup-and-restore-for-mariadb-columnstore-110-onwards/](https://mariadb.com/kb/en/library/backup-and-restore-for-mariadb-columnstore-110-onwards/)

### MariaDB ColumnStore Connectors

#### ODBC Connector

How to Install the ODBC Connector:

[https://mariadb.com/kb/en/library/about-mariadb-connector-odbc/](https://mariadb.com/kb/en/library/about-mariadb-connector-odbc/)

Additional Information on the ODBC connector:

[https://mariadb.com/kb/en/library/mariadb-connector-odbc/](https://mariadb.com/kb/en/library/mariadb-connector-odbc/)

#### Java Connector

How to Install the Java Connector:

[https://mariadb.com/kb/en/library/getting-started-with-the-java-connector/](https://mariadb.com/kb/en/library/getting-started-with-the-java-connector/)

Additional information on the Java Connector:

[https://mariadb.com/kb/en/library/mariadb-connector-j/](https://mariadb.com/kb/en/library/mariadb-connector-j/)

How to Install on the MariaDB MaxScale:

[https://mariadb.com/kb/en/mariadb-enterprise/mariadb-maxscale-22-installing-mariadb-maxscale-using-a-tarball/](https://mariadb.com/kb/en/mariadb-enterprise/mariadb-maxscale-22-installing-mariadb-maxscale-using-a-tarball/)

## Suse 12 Binary Package

Once the package is downloaded, it can be untarred with the following command:

```sql
tar zxvf mariadb-columnstore-x.x.x-x-sles12.x86_64.bin.tar.gz
```

This is the contents of the installed package:

- MariaDB-ColumnStore
<ul start="1"><li>mariadb-columnstore-1.1.3-1-sles12.x86_64.bin.tar.gz
</li></ul>
- MariaDB-ColumnStore-Tools
<ul start="1"><li>mariadb-columnstore-tools-1.1.3-1.bin.tar.gz
</li></ul>
- MariaDB-Connectors/
<ul start="1"><li>mariadb-java-client-2.2.2.jar
</li></ul>
- MariaDB-MaxScale/
<ul start="1"><li>maxscale-2.2.2-1.sles.12.tar.gz
</li><li>maxscale-cdc-connector-2.2.2-1.sles.12.x86_64.rpm
</li></ul>

## Suse 12 Binary Package installation

### MariaDB ColumnStore

Follow the instructions for installing MariaDB Columnstore binary package

[https://mariadb.com/kb/en/library/preparing-for-columnstore-installation-11x/#choosing-the-type-of-initial-downloadinstall](https://mariadb.com/kb/en/library/preparing-for-columnstore-installation-11x/#choosing-the-type-of-initial-downloadinstall)

### MariaDB ColumnStore Tools

Follow the instructions for installing MariaDB Columnstore Tools

[https://mariadb.com/kb/en/library/backup-and-restore-for-mariadb-columnstore-110-onwards/](https://mariadb.com/kb/en/library/backup-and-restore-for-mariadb-columnstore-110-onwards/)

### MariaDB ColumnStore Connectors

#### ODBC Connector

How to Install the ODBC Connector:

[https://mariadb.com/kb/en/library/about-mariadb-connector-odbc/](https://mariadb.com/kb/en/library/about-mariadb-connector-odbc/)

Additional Information on the ODBC connector:

[https://mariadb.com/kb/en/library/mariadb-connector-odbc/](https://mariadb.com/kb/en/library/mariadb-connector-odbc/)

#### Java Connector

How to Install the Java Connector:

[https://mariadb.com/kb/en/library/getting-started-with-the-java-connector/](https://mariadb.com/kb/en/library/getting-started-with-the-java-connector/)

Additional information on the Java Connector:

[https://mariadb.com/kb/en/library/mariadb-connector-j/](https://mariadb.com/kb/en/library/mariadb-connector-j/)

### MariaDB MaxScale

How to Install on the MariaDB MaxScale:

[https://mariadb.com/kb/en/mariadb-enterprise/mariadb-maxscale-22-installing-mariadb-maxscale-using-a-tarball/](https://mariadb.com/kb/en/mariadb-enterprise/mariadb-maxscale-22-installing-mariadb-maxscale-using-a-tarball/)

# Debian 8 (jessie)

## Debian 8 DEB package

Once the package is downloaded, it can be untarred with the following command:

```sql
tar zxvf mariadb-columnstore-1.1.3-1-jessie.amd64.deb.tar.gz
```

This is the contents of the installed package:

- MariaDB-ColumnStore
<ul start="1"><li>mariadb-columnstore-1.1.3-1-jessie.amd64.rpm.tar.gz
</li></ul>
- MariaDB-ColumnStore-API/
<ul start="1"><li>mariadb-columnstore-api-1.1.3-1-x86_64.jessie.deb
</li></ul>
- MariaDB-ColumnStore-Tools
<ul start="1"><li>mariadb-columnstore-tools-1.1.3-1.amd64.deb
</li></ul>
- MariaDB-Connectors/
<ul start="1"><li>mariadb-connector-[ODBC-3](https://jira.mariadb.org/browse/ODBC-3).0.2-ga-debian-i686.tar.gz
</li><li>mariadb-java-client-2.2.2.jar
</li></ul>
- MariaDB-Data-Adapters/
<ul start="1"><li>mariadb-columnstore-kafka-adapters-1.1.3-1-x86_64-jessie.deb
</li><li>mariadb-columnstore-maxscale-cdc-adapters-1.1.3-1-x86_64-jessie.deb
</li></ul>
- MariaDB-MaxScale/
<ul start="1"><li>maxscale-2.2.2-1.debian.jessie.x86_64.deb
</li><li>maxscale-cdc-connector-2.2.2-1.debian.jessie.x86_64.deb
</li></ul>

## Debian 8 DEB package installation

### MariaDB ColumnStore

Follow the instructions for installing MariaDB Columnstore DEBs

[https://mariadb.com/kb/en/library/preparing-for-columnstore-installation-11x/#choosing-the-type-of-initial-downloadinstall](https://mariadb.com/kb/en/library/preparing-for-columnstore-installation-11x/#choosing-the-type-of-initial-downloadinstall)

### MariaDB ColumnStore API (Bulk Write SDK)

Follow the instructions for installing MariaDB Columnstore API DEB

[https://mariadb.com/kb/en/library/columnstore-bulk-write-sdk/](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-data-ingestion/columnstore-bulk-write-sdk)

### MariaDB ColumnStore Tools

Follow the instructions for installing MariaDB Columnstore Tools

[https://mariadb.com/kb/en/library/backup-and-restore-for-mariadb-columnstore-110-onwards/](https://mariadb.com/kb/en/library/backup-and-restore-for-mariadb-columnstore-110-onwards/)

### MariaDB ColumnStore Connectors

#### ODBC Connector

How to Install the ODBC Connector:

[https://mariadb.com/kb/en/library/about-mariadb-connector-odbc/](https://mariadb.com/kb/en/library/about-mariadb-connector-odbc/)

Additional Information on the ODBC connector:

[https://mariadb.com/kb/en/library/mariadb-connector-odbc/](https://mariadb.com/kb/en/library/mariadb-connector-odbc/)

#### Java Connector

How to Install the Java Connector:

[https://mariadb.com/kb/en/library/getting-started-with-the-java-connector/](https://mariadb.com/kb/en/library/getting-started-with-the-java-connector/)

Additional information on the Java Connector:

[https://mariadb.com/kb/en/library/mariadb-connector-j/](https://mariadb.com/kb/en/library/mariadb-connector-j/)

### MariaDB ColumnStore Data Adapters

How to Install on the Data Adapters:

[https://mariadb.com/kb/en/library/columnstore-streaming-data-adapters/](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-data-ingestion/columnstore-streaming-data-adapters)

### MariaDB MaxScale

How to Install on the MariaDB MaxScale:

[https://mariadb.com/kb/en/mariadb-enterprise/mariadb-maxscale-22-installing-mariadb-maxscale-using-a-tarball/](https://mariadb.com/kb/en/mariadb-enterprise/mariadb-maxscale-22-installing-mariadb-maxscale-using-a-tarball/)

## Debian 8 Binary Package

Once the package is downloaded, it can be untarred with the following command:

```sql
tar zxvf mariadb-columnstore-x.x.x-x-jessie.x86_64.bin.tar.gz
```

This is the contents of the installed package:

- MariaDB-ColumnStore
<ul start="1"><li>mariadb-columnstore-1.1.3-1-jessie.amd64.bin.tar.gz
</li></ul>
- MariaDB-ColumnStore-API/
<ul start="1"><li>mariadb-columnstore-api-1.1.3-1-x86_64-jessie.deb
</li></ul>
- MariaDB-ColumnStore-Tools
<ul start="1"><li>mariadb-columnstore-tools-1.1.3-1.tar.gz
</li></ul>
- MariaDB-Connectors/
<ul start="1"><li>mariadb-connector-[ODBC-3](https://jira.mariadb.org/browse/ODBC-3).0.2-ga-debian-i686.tar.gz
</li><li>mariadb-java-client-2.2.2.jar
</li></ul>
- MariaDB-Data-Adapters/
<ul start="1"><li>mariadb-columnstore-kafka-adapters-1.1.3-1-x86_64-jessie.deb
</li><li>mariadb-columnstore-maxscale-cdc-adapters-1.1.3-1-x86_64-jessie.deb
</li></ul>
- MariaDB-MaxScale/
<ul start="1"><li>maxscale-2.2.2-1.debian.jessie.tar.gz
</li><li>maxscale-cdc-connector-2.2.2-1.debian.jessie.x86_64.deb
</li></ul>

## Debian 8 Binary Package installation

### MariaDB ColumnStore

Follow the instructions for installing MariaDB Columnstore binary package

[https://mariadb.com/kb/en/library/preparing-for-columnstore-installation-11x/#choosing-the-type-of-initial-downloadinstall](https://mariadb.com/kb/en/library/preparing-for-columnstore-installation-11x/#choosing-the-type-of-initial-downloadinstall)

### MariaDB ColumnStore API (Bulk Write SDK)

Follow the instructions for installing MariaDB Columnstore API RPM

[https://mariadb.com/kb/en/library/columnstore-bulk-write-sdk/](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-data-ingestion/columnstore-bulk-write-sdk)

### MariaDB ColumnStore Tools

Follow the instructions for installing MariaDB Columnstore Tools

[https://mariadb.com/kb/en/library/backup-and-restore-for-mariadb-columnstore-110-onwards/](https://mariadb.com/kb/en/library/backup-and-restore-for-mariadb-columnstore-110-onwards/)

### MariaDB ColumnStore Connectors

#### ODBC Connector

How to Install the ODBC Connector:

[https://mariadb.com/kb/en/library/about-mariadb-connector-odbc/](https://mariadb.com/kb/en/library/about-mariadb-connector-odbc/)

Additional Information on the ODBC connector:

[https://mariadb.com/kb/en/library/mariadb-connector-odbc/](https://mariadb.com/kb/en/library/mariadb-connector-odbc/)

#### Java Connector

How to Install the Java Connector:

[https://mariadb.com/kb/en/library/getting-started-with-the-java-connector/](https://mariadb.com/kb/en/library/getting-started-with-the-java-connector/)

Additional information on the Java Connector:

[https://mariadb.com/kb/en/library/mariadb-connector-j/](https://mariadb.com/kb/en/library/mariadb-connector-j/)

### MariaDB ColumnStore Data Adapters

How to Install on the Data Adapters:

[https://mariadb.com/kb/en/library/columnstore-streaming-data-adapters/](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-data-ingestion/columnstore-streaming-data-adapters)

### MariaDB MaxScale

How to Install on the MariaDB MaxScale:

[https://mariadb.com/kb/en/mariadb-enterprise/mariadb-maxscale-22-installing-mariadb-maxscale-using-a-tarball/](https://mariadb.com/kb/en/mariadb-enterprise/mariadb-maxscale-22-installing-mariadb-maxscale-using-a-tarball/)

# Debian 9 (stretch)

## Debian 9 DEB package

Once the package is downloaded, it can be untarred with the following command:

```sql
tar zxvf mariadb-columnstore-1.1.3-1-stretch.amd64.deb.tar.gz
```

This is the contents of the installed package:

- MariaDB-ColumnStore
<ul start="1"><li>mariadb-columnstore-1.1.3-1-stretch.amd64.rpm.tar.gz
</li></ul>
- MariaDB-ColumnStore-API/
<ul start="1"><li>mariadb-columnstore-api-1.1.3-1-x86_64.stretch.deb
</li></ul>
- MariaDB-ColumnStore-Tools
<ul start="1"><li>mariadb-columnstore-tools-1.1.3-1.amd64.deb
</li></ul>
- MariaDB-Connectors/
<ul start="1"><li>mariadb-connector-[ODBC-3](https://jira.mariadb.org/browse/ODBC-3).0.2-ga-debian-i686.tar.gz
</li><li>mariadb-java-client-2.2.2.jar
</li></ul>
- MariaDB-Data-Adapters/
<ul start="1"><li>mariadb-columnstore-kafka-adapters-1.1.3-1-x86_64-stretch.deb
</li><li>mariadb-columnstore-maxscale-cdc-adapters-1.1.3-1-x86_64-stretch.deb
</li></ul>
- MariaDB-MaxScale/
<ul start="1"><li>maxscale-2.2.2-1.debian.stretch.x86_64.deb
</li><li>maxscale-cdc-connector-2.2.2-1.debian.stretch.x86_64.deb
</li></ul>

## Debian 9 DEB package installation

### MariaDB ColumnStore

Follow the instructions for installing MariaDB Columnstore DEBs

[https://mariadb.com/kb/en/library/preparing-for-columnstore-installation-11x/#choosing-the-type-of-initial-downloadinstall](https://mariadb.com/kb/en/library/preparing-for-columnstore-installation-11x/#choosing-the-type-of-initial-downloadinstall)

### MariaDB ColumnStore API (Bulk Write SDK)

Follow the instructions for installing MariaDB Columnstore API DEB

[https://mariadb.com/kb/en/library/columnstore-bulk-write-sdk/](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-data-ingestion/columnstore-bulk-write-sdk)

### MariaDB ColumnStore Tools

Follow the instructions for installing MariaDB Columnstore Tools

[https://mariadb.com/kb/en/library/backup-and-restore-for-mariadb-columnstore-110-onwards/](https://mariadb.com/kb/en/library/backup-and-restore-for-mariadb-columnstore-110-onwards/)

### MariaDB ColumnStore Connectors

#### ODBC Connector

How to Install the ODBC Connector:

[https://mariadb.com/kb/en/library/about-mariadb-connector-odbc/](https://mariadb.com/kb/en/library/about-mariadb-connector-odbc/)

Additional Information on the ODBC connector:

[https://mariadb.com/kb/en/library/mariadb-connector-odbc/](https://mariadb.com/kb/en/library/mariadb-connector-odbc/)

#### Java Connector

How to Install the Java Connector:

[https://mariadb.com/kb/en/library/getting-started-with-the-java-connector/](https://mariadb.com/kb/en/library/getting-started-with-the-java-connector/)

Additional information on the Java Connector:

[https://mariadb.com/kb/en/library/mariadb-connector-j/](https://mariadb.com/kb/en/library/mariadb-connector-j/)

### MariaDB ColumnStore Data Adapters

How to Install on the Data Adapters:

[https://mariadb.com/kb/en/library/columnstore-streaming-data-adapters/](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-data-ingestion/columnstore-streaming-data-adapters)

### MariaDB MaxScale

How to Install on the MariaDB MaxScale:

[https://mariadb.com/kb/en/mariadb-enterprise/mariadb-maxscale-22-installing-mariadb-maxscale-using-a-tarball/](https://mariadb.com/kb/en/mariadb-enterprise/mariadb-maxscale-22-installing-mariadb-maxscale-using-a-tarball/)

## Debian 9 Binary Package

Once the package is downloaded, it can be untarred with the following command:

```sql
tar zxvf mariadb-columnstore-x.x.x-x-stretch.x86_64.bin.tar.gz
```

This is the contents of the installed package:

- MariaDB-ColumnStore
<ul start="1"><li>mariadb-columnstore-1.1.3-1-stretch.amd64.bin.tar.gz
</li></ul>
- MariaDB-ColumnStore-API/
<ul start="1"><li>mariadb-columnstore-api-1.1.3-1-x86_64-stretch.deb
</li></ul>
- MariaDB-ColumnStore-Tools
<ul start="1"><li>mariadb-columnstore-tools-1.1.3-1.tar.gz
</li></ul>
- MariaDB-Connectors/
<ul start="1"><li>mariadb-connector-[ODBC-3](https://jira.mariadb.org/browse/ODBC-3).0.2-ga-debian-i686.tar.gz
</li><li>mariadb-java-client-2.2.2.jar
</li></ul>
- MariaDB-Data-Adapters/
<ul start="1"><li>mariadb-columnstore-kafka-adapters-1.1.3-1-x86_64-stretch.deb
</li><li>mariadb-columnstore-maxscale-cdc-adapters-1.1.3-1-x86_64-stretch.deb
</li></ul>
- MariaDB-MaxScale/
<ul start="1"><li>maxscale-2.2.2-1.debian.stretch.tar.gz
</li><li>maxscale-cdc-connector-2.2.2-1.debian.stretch.x86_64.deb
</li></ul>

## Debian 9 Binary Package installation

### MariaDB ColumnStore

Follow the instructions for installing MariaDB Columnstore binary package

[https://mariadb.com/kb/en/library/preparing-for-columnstore-installation-11x/#choosing-the-type-of-initial-downloadinstall](https://mariadb.com/kb/en/library/preparing-for-columnstore-installation-11x/#choosing-the-type-of-initial-downloadinstall)

### MariaDB ColumnStore API (Bulk Write SDK)

Follow the instructions for installing MariaDB Columnstore API RPM

[https://mariadb.com/kb/en/library/columnstore-bulk-write-sdk/](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-data-ingestion/columnstore-bulk-write-sdk)

### MariaDB ColumnStore Tools

Follow the instructions for installing MariaDB Columnstore Tools

[https://mariadb.com/kb/en/library/backup-and-restore-for-mariadb-columnstore-110-onwards/](https://mariadb.com/kb/en/library/backup-and-restore-for-mariadb-columnstore-110-onwards/)

### MariaDB ColumnStore Connectors

#### ODBC Connector

How to Install the ODBC Connector:

[https://mariadb.com/kb/en/library/about-mariadb-connector-odbc/](https://mariadb.com/kb/en/library/about-mariadb-connector-odbc/)

Additional Information on the ODBC connector:

[https://mariadb.com/kb/en/library/mariadb-connector-odbc/](https://mariadb.com/kb/en/library/mariadb-connector-odbc/)

#### Java Connector

How to Install the Java Connector:

[https://mariadb.com/kb/en/library/getting-started-with-the-java-connector/](https://mariadb.com/kb/en/library/getting-started-with-the-java-connector/)

Additional information on the Java Connector:

[https://mariadb.com/kb/en/library/mariadb-connector-j/](https://mariadb.com/kb/en/library/mariadb-connector-j/)

### MariaDB ColumnStore Data Adapters

How to Install on the Data Adapters:

[https://mariadb.com/kb/en/library/columnstore-streaming-data-adapters/](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-data-ingestion/columnstore-streaming-data-adapters)

### MariaDB MaxScale

How to Install on the MariaDB MaxScale:

[https://mariadb.com/kb/en/mariadb-enterprise/mariadb-maxscale-22-installing-mariadb-maxscale-using-a-tarball/](https://mariadb.com/kb/en/mariadb-enterprise/mariadb-maxscale-22-installing-mariadb-maxscale-using-a-tarball/)

# Ubuntu 16 (xenial)

## Ubuntu 16 DEB package

Once the package is downloaded, it can be untarred with the following command:

```sql
tar zxvf mariadb-columnstore-1.1.3-1-xenial.amd64.deb.tar.gz
```

This is the contents of the installed package:

- MariaDB-ColumnStore
<ul start="1"><li>mariadb-columnstore-1.1.3-1-xenial.amd64.rpm.tar.gz
</li></ul>
- MariaDB-ColumnStore-API/
<ul start="1"><li>mariadb-columnstore-api-1.1.3-1-x86_64.xenial.deb
</li></ul>
- MariaDB-ColumnStore-Tools
<ul start="1"><li>mariadb-columnstore-tools-1.1.3-1.amd64.deb
</li></ul>
- MariaDB-Connectors/
<ul start="1"><li>mariadb-connector-[ODBC-3](https://jira.mariadb.org/browse/ODBC-3).0.2-ga-debian-i686.tar.gz
</li><li>mariadb-java-client-2.2.2.jar
</li></ul>
- MariaDB-Data-Adapters/
<ul start="1"><li>mariadb-columnstore-kafka-adapters-1.1.3-1-x86_64-xenial.deb
</li><li>mariadb-columnstore-maxscale-cdc-adapters-1.1.3-1-x86_64-xenial.deb
</li></ul>
- MariaDB-MaxScale/
<ul start="1"><li>maxscale-2.2.2-1.debian.xenial.x86_64.deb
</li><li>maxscale-cdc-connector-2.2.2-1.debian.xenial.x86_64.deb
</li></ul>

## Ubuntu 16 DEB package installation

### MariaDB ColumnStore

Follow the instructions for installing MariaDB Columnstore DEBs

[https://mariadb.com/kb/en/library/preparing-for-columnstore-installation-11x/#choosing-the-type-of-initial-downloadinstall](https://mariadb.com/kb/en/library/preparing-for-columnstore-installation-11x/#choosing-the-type-of-initial-downloadinstall)

### MariaDB ColumnStore API (Bulk Write SDK)

Follow the instructions for installing MariaDB Columnstore API DEB

[https://mariadb.com/kb/en/library/columnstore-bulk-write-sdk/](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-data-ingestion/columnstore-bulk-write-sdk)

### MariaDB ColumnStore Tools

Follow the instructions for installing MariaDB Columnstore Tools

[https://mariadb.com/kb/en/library/backup-and-restore-for-mariadb-columnstore-110-onwards/](https://mariadb.com/kb/en/library/backup-and-restore-for-mariadb-columnstore-110-onwards/)

### MariaDB ColumnStore Connectors

#### ODBC Connector

How to Install the ODBC Connector:

[https://mariadb.com/kb/en/library/about-mariadb-connector-odbc/](https://mariadb.com/kb/en/library/about-mariadb-connector-odbc/)

Additional Information on the ODBC connector:

[https://mariadb.com/kb/en/library/mariadb-connector-odbc/](https://mariadb.com/kb/en/library/mariadb-connector-odbc/)

#### Java Connector

How to Install the Java Connector:

[https://mariadb.com/kb/en/library/getting-started-with-the-java-connector/](https://mariadb.com/kb/en/library/getting-started-with-the-java-connector/)

Additional information on the Java Connector:

[https://mariadb.com/kb/en/library/mariadb-connector-j/](https://mariadb.com/kb/en/library/mariadb-connector-j/)

### MariaDB ColumnStore Data Adapters

How to Install on the Data Adapters:

[https://mariadb.com/kb/en/library/columnstore-streaming-data-adapters/](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-data-ingestion/columnstore-streaming-data-adapters)

### MariaDB MaxScale

How to Install on the MariaDB MaxScale:

[https://mariadb.com/kb/en/mariadb-enterprise/mariadb-maxscale-22-installing-mariadb-maxscale-using-a-tarball/](https://mariadb.com/kb/en/mariadb-enterprise/mariadb-maxscale-22-installing-mariadb-maxscale-using-a-tarball/)

## Ubuntu 16 Binary Package

Once the package is downloaded, it can be untarred with the following command:

```sql
tar zxvf mariadb-columnstore-x.x.x-x-xenial.adm64.bin.tar.gz
```

This is the contents of the installed package:

- MariaDB-ColumnStore
<ul start="1"><li>mariadb-columnstore-1.1.3-1-xenial.amd64.bin.tar.gz
</li></ul>
- MariaDB-ColumnStore-API/
<ul start="1"><li>mariadb-columnstore-api-1.1.3-1-x86_64-xenial.deb
</li></ul>
- MariaDB-ColumnStore-Tools
<ul start="1"><li>mariadb-columnstore-tools-1.1.3-1.tar.gz
</li></ul>
- MariaDB-Connectors
<ul start="1"><li>mariadb-connector-[ODBC-3](https://jira.mariadb.org/browse/ODBC-3).0.2-ga-debian-i686.tar.gz
</li><li>mariadb-java-client-2.2.2.jar
</li></ul>
- MariaDB-Data-Adapters
<ul start="1"><li>mariadb-columnstore-kafka-adapters-1.1.3-1-x86_64-xenial.deb
</li><li>mariadb-columnstore-maxscale-cdc-adapters-1.1.3-1-x86_64-xenial.deb
</li></ul>
- MariaDB-MaxScale
<ul start="1"><li>maxscale-2.2.2-1.debian.xenial.tar.gz
</li><li>maxscale-cdc-connector-2.2.2-1.debian.xenial.x86_64.deb
</li></ul>

## Ubuntu 16 Binary Package installation

### MariaDB ColumnStore

Follow the instructions for installing MariaDB Columnstore binary package

[https://mariadb.com/kb/en/library/preparing-for-columnstore-installation-11x/#choosing-the-type-of-initial-downloadinstall](https://mariadb.com/kb/en/library/preparing-for-columnstore-installation-11x/#choosing-the-type-of-initial-downloadinstall)

### MariaDB ColumnStore API (Bulk Write SDK)

Follow the instructions for installing MariaDB Columnstore API RPM

[https://mariadb.com/kb/en/library/columnstore-bulk-write-sdk/](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-data-ingestion/columnstore-bulk-write-sdk)

### MariaDB ColumnStore Tools

Follow the instructions for installing MariaDB Columnstore Tools

[https://mariadb.com/kb/en/library/backup-and-restore-for-mariadb-columnstore-110-onwards/](https://mariadb.com/kb/en/library/backup-and-restore-for-mariadb-columnstore-110-onwards/)

### MariaDB ColumnStore Connectors

#### ODBC Connector

How to Install the ODBC Connector:

[https://mariadb.com/kb/en/library/about-mariadb-connector-odbc/](https://mariadb.com/kb/en/library/about-mariadb-connector-odbc/)

Additional Information on the ODBC connector:

[https://mariadb.com/kb/en/library/mariadb-connector-odbc/](https://mariadb.com/kb/en/library/mariadb-connector-odbc/)

#### Java Connector

How to Install the Java Connector:

[https://mariadb.com/kb/en/library/getting-started-with-the-java-connector/](https://mariadb.com/kb/en/library/getting-started-with-the-java-connector/)

Additional information on the Java Connector:

[https://mariadb.com/kb/en/library/mariadb-connector-j/](https://mariadb.com/kb/en/library/mariadb-connector-j/)

### MariaDB ColumnStore Data Adapters

How to Install on the Data Adapters:

[https://mariadb.com/kb/en/library/columnstore-streaming-data-adapters/](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-data-ingestion/columnstore-streaming-data-adapters)

### MariaDB MaxScale

How to Install on the MariaDB MaxScale:

[https://mariadb.com/kb/en/mariadb-enterprise/mariadb-maxscale-22-installing-mariadb-maxscale-using-a-tarball/](https://mariadb.com/kb/en/mariadb-enterprise/mariadb-maxscale-22-installing-mariadb-maxscale-using-a-tarball/)

# Ubuntu 18 (bionic)

## Ubuntu 18 DEB package

Once the package is downloaded, it can be untarred with the following command:

```sql
tar zxvf mariadb-columnstore-1.1.3-1-bionic.amd64.deb.tar.gz
```

This is the contents of the installed package:

- MariaDB-ColumnStore
<ul start="1"><li>mariadb-columnstore-1.1.3-1-bionic.amd64.rpm.tar.gz
</li></ul>
- MariaDB-ColumnStore-API
<ul start="1"><li>mariadb-columnstore-api-1.1.3-1-x86_64.bionic.deb
</li></ul>
- MariaDB-ColumnStore-Tools
<ul start="1"><li>mariadb-columnstore-tools-1.1.3-1.amd64.deb
</li></ul>
- MariaDB-Connectors
<ul start="1"><li>mariadb-connector-[ODBC-3](https://jira.mariadb.org/browse/ODBC-3).0.2-ga-debian-i686.tar.gz
</li><li>mariadb-java-client-2.2.2.jar
</li></ul>
- MariaDB-Data-Adapters
<ul start="1"><li>mariadb-columnstore-kafka-adapters-1.1.3-1-x86_64-bionic.deb
</li><li>mariadb-columnstore-maxscale-cdc-adapters-1.1.3-1-x86_64-bionic.deb
</li></ul>
- MariaDB-MaxScale
<ul start="1"><li>maxscale-2.2.2-1.debian.bionic.x86_64.deb
</li><li>maxscale-cdc-connector-2.2.2-1.debian.bionic.x86_64.deb
</li></ul>

## Ubuntu 18 DEB package installation

### MariaDB ColumnStore

Follow the instructions for installing MariaDB Columnstore DEBs

[https://mariadb.com/kb/en/library/preparing-for-columnstore-installation-11x/#choosing-the-type-of-initial-downloadinstall](https://mariadb.com/kb/en/library/preparing-for-columnstore-installation-11x/#choosing-the-type-of-initial-downloadinstall)

### MariaDB ColumnStore API (Bulk Write SDK)

Follow the instructions for installing MariaDB Columnstore API DEB

[https://mariadb.com/kb/en/library/columnstore-bulk-write-sdk/](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-data-ingestion/columnstore-bulk-write-sdk)

### MariaDB ColumnStore Tools

Follow the instructions for installing MariaDB Columnstore Tools

[https://mariadb.com/kb/en/library/backup-and-restore-for-mariadb-columnstore-110-onwards/](https://mariadb.com/kb/en/library/backup-and-restore-for-mariadb-columnstore-110-onwards/)

### MariaDB ColumnStore Connectors

#### ODBC Connector

How to Install the ODBC Connector:

[https://mariadb.com/kb/en/library/about-mariadb-connector-odbc/](https://mariadb.com/kb/en/library/about-mariadb-connector-odbc/)

Additional Information on the ODBC connector:

[https://mariadb.com/kb/en/library/mariadb-connector-odbc/](https://mariadb.com/kb/en/library/mariadb-connector-odbc/)

#### Java Connector

How to Install the Java Connector:

[https://mariadb.com/kb/en/library/getting-started-with-the-java-connector/](https://mariadb.com/kb/en/library/getting-started-with-the-java-connector/)

Additional information on the Java Connector:

[https://mariadb.com/kb/en/library/mariadb-connector-j/](https://mariadb.com/kb/en/library/mariadb-connector-j/)

### MariaDB ColumnStore Data Adapters

How to Install on the Data Adapters:

[https://mariadb.com/kb/en/library/columnstore-streaming-data-adapters/](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-data-ingestion/columnstore-streaming-data-adapters)

### MariaDB MaxScale

How to Install on the MariaDB MaxScale:

[https://mariadb.com/kb/en/mariadb-enterprise/mariadb-maxscale-22-installing-mariadb-maxscale-using-a-tarball/](https://mariadb.com/kb/en/mariadb-enterprise/mariadb-maxscale-22-installing-mariadb-maxscale-using-a-tarball/)

## Ubuntu 18 Binary Package

Once the package is downloaded, it can be untarred with the following command:

```sql
tar zxvf mariadb-columnstore-x.x.x-x-bionic.adm64.bin.tar.gz
```

This is the contents of the installed package:

- MariaDB-ColumnStore
<ul start="1"><li>mariadb-columnstore-1.1.3-1-bionic.amd64.bin.tar.gz
</li></ul>
- MariaDB-ColumnStore-API/
<ul start="1"><li>mariadb-columnstore-api-1.1.3-1-x86_64-bionic.deb
</li></ul>
- MariaDB-ColumnStore-Tools
<ul start="1"><li>mariadb-columnstore-tools-1.1.3-1.tar.gz
</li></ul>
- MariaDB-Connectors
<ul start="1"><li>mariadb-connector-[ODBC-3](https://jira.mariadb.org/browse/ODBC-3).0.2-ga-debian-i686.tar.gz
</li><li>mariadb-java-client-2.2.2.jar
</li></ul>
- MariaDB-Data-Adapters
<ul start="1"><li>mariadb-columnstore-kafka-adapters-1.1.3-1-x86_64-bionic.deb
</li><li>mariadb-columnstore-maxscale-cdc-adapters-1.1.3-1-x86_64-bionic.deb
</li></ul>
- MariaDB-MaxScale
<ul start="1"><li>maxscale-2.2.2-1.debian.bionic.tar.gz
</li><li>maxscale-cdc-connector-2.2.2-1.debian.bionic.x86_64.deb
</li></ul>

## Ubuntu 18 Binary Package installation

### MariaDB ColumnStore

Follow the instructions for installing MariaDB Columnstore binary package

[https://mariadb.com/kb/en/library/preparing-for-columnstore-installation-11x/#choosing-the-type-of-initial-downloadinstall](https://mariadb.com/kb/en/library/preparing-for-columnstore-installation-11x/#choosing-the-type-of-initial-downloadinstall)

### MariaDB ColumnStore API (Bulk Write SDK)

Follow the instructions for installing MariaDB Columnstore API RPM

[https://mariadb.com/kb/en/library/columnstore-bulk-write-sdk/](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-data-ingestion/columnstore-bulk-write-sdk)

### MariaDB ColumnStore Tools

Follow the instructions for installing MariaDB Columnstore Tools

[https://mariadb.com/kb/en/library/backup-and-restore-for-mariadb-columnstore-110-onwards/](https://mariadb.com/kb/en/library/backup-and-restore-for-mariadb-columnstore-110-onwards/)

### MariaDB ColumnStore Connectors

#### ODBC Connector

How to Install the ODBC Connector:

[https://mariadb.com/kb/en/library/about-mariadb-connector-odbc/](https://mariadb.com/kb/en/library/about-mariadb-connector-odbc/)

Additional Information on the ODBC connector:

[https://mariadb.com/kb/en/library/mariadb-connector-odbc/](https://mariadb.com/kb/en/library/mariadb-connector-odbc/)

#### Java Connector

How to Install the Java Connector:

[https://mariadb.com/kb/en/library/getting-started-with-the-java-connector/](https://mariadb.com/kb/en/library/getting-started-with-the-java-connector/)

Additional information on the Java Connector:

[https://mariadb.com/kb/en/library/mariadb-connector-j/](https://mariadb.com/kb/en/library/mariadb-connector-j/)

### MariaDB ColumnStore Data Adapters

How to Install on the Data Adapters:

[https://mariadb.com/kb/en/library/columnstore-streaming-data-adapters/](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-data-ingestion/columnstore-streaming-data-adapters)

### MariaDB MaxScale

How to Install on the MariaDB MaxScale:

[https://mariadb.com/kb/en/mariadb-enterprise/mariadb-maxscale-22-installing-mariadb-maxscale-using-a-tarball/](https://mariadb.com/kb/en/mariadb-enterprise/mariadb-maxscale-22-installing-mariadb-maxscale-using-a-tarball/)