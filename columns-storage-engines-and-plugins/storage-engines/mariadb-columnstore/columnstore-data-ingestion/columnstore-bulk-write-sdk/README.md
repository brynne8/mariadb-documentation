# ColumnStore Bulk Write SDK

Starting with MariaDB ColumnStore 1.4, the Bulk Write SDK is deprecated, and it should not be used for loading data. The more modern recommendations are:

- Load data using [cpimport](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-data-ingestion/columnstore-bulk-data-loading/).
- Or connect to MariaDB ColumnStore using a standard MariaDB [client](/clients-utilities/) or [connector](/kb/en/connectors/), and then load data using [LOAD DATA INFILE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/load-data-into-tables-or-index/load-data-infile/).

# Introduction

Starting with MariaDB ColumnStore 1.1 a C++ SDK is available which supports bulk write into ColumnStore. Conceptually this is an API version of cpimport.  The SDK is intended to be integrated by custom code and adapters to enable easier publishing of data into ColumnStore.

The API is licensed under LGPLv3.

# Getting Started

Prebuilt binary packages may be downloaded [here](https://mariadb.com/downloads/mariadb-ax/data-adapters) or you can build from scratch from [here](https://github.com/mariadb-corporation/mariadb-columnstore-api). To build from scratch please follow the instructions in GitHub's Readme.md.

## Installation

### From our repositories

For the installation from our RPM and deb repositories please have a look at this documents:

<strong>Bulk Write SDK 1.1.X</strong>

[https://mariadb.com/kb/en/library/installing-mariadb-ax-mariadb-columnstore-from-the-package-repositories/](https://mariadb.com/kb/en/library/installing-mariadb-ax-mariadb-columnstore-from-the-package-repositories/)

<strong>Bulk Write SDK 1.2.X</strong>

[https://mariadb.com/kb/en/library/installing-mariadb-ax-mariadb-columnstore-from-the-package-repositories-122/](https://mariadb.com/kb/en/library/installing-mariadb-ax-mariadb-columnstore-from-the-package-repositories-122/)

### RHEL / CentOS 7 Package

The following libraries need to be installed on the system for the package install:

```sql
yum install epel-release
yum install libuv libxml2 snappy python36
```

The API rpm can be installed via:

```sql
rpm -ivh mariadb-columnstore-api*.rpm
```

#### Bulk Write SDK 1.2.2

Starting with version 1.2.2 the bulk write SDK is split up into its components which be installed separately. Packages are:

<table><tbody><tr><th>package name</th><th>description</th></tr>
<tr><td>mariadb-columnstore-api-1.2.*-x86_64-centos7-cpp.rpm</td><td>The base C++ part needed to execute programs</td></tr>
<tr><td>mariadb-columnstore-api-1.2.*-x86_64-centos7-cpp-devel.rpm</td><td>The C++ development part needed to build programs</td></tr>
<tr><td>mariadb-columnstore-api-1.2.*-x86_64-centos7-java.rpm</td><td>The Java part needed to use the API in Java</td></tr>
<tr><td>mariadb-columnstore-api-1.2.*-x86_64-centos7-python.rpm</td><td>The Python part needed to use the API in Python 2</td></tr>
<tr><td>mariadb-columnstore-api-1.2.*-x86_64-centos7-python3.rpm</td><td>The Python part needed to use the API in Python 3</td></tr>
<tr><td>mariadb-columnstore-api-1.2.*-x86_64-centos7-spark.rpm</td><td>The Scala/Spark part including ColumnStoreExporter</td></tr>
<tr><td>mariadb-columnstore-api-1.2.*-x86_64-centos7-pyspark.rpm</td><td>The Python 2/Spark part including ColumnStoreExporter</td></tr>
<tr><td>mariadb-columnstore-api-1.2.*-x86_64-centos7-pyspark3.rpm</td><td>The Python 3/Spark part including ColumnStoreExporter</td></tr>
</tbody></table>

### Ubuntu 16 / Debian 9 Package

The following libraries need to be installed on the system for the package install:

```sql
apt-get install libuv1 libxml2 libsnappy1v5
```

The API deb package can be installed via:

```sql
dpkg -i mariadb-columnstore-api*.deb
```

#### Bulk Write SDK 1.2.2

Starting with version 1.2.2 the bulk write SDK is split up into its components which can be installed separately. Packages are:

<table><tbody><tr><th>package name</th><th>description</th></tr>
<tr><td>maraidb-columnstore-api-cpp_1.2.*_amd64.deb</td><td>The base C++ part needed to execute programs</td></tr>
<tr><td>mariadb-columnstore-api-cpp-devel_1.2.*_amd64.deb</td><td>The C++ development part needed to build programs</td></tr>
<tr><td>mariadb-columnstore-api-java_1.2.*_amd64.deb</td><td>The Java part needed to use the API in Java</td></tr>
<tr><td>mariadb-columnstore-api-python_1.2.*_amd64.deb</td><td>The Python part needed to use the API in Python 2</td></tr>
<tr><td>mariadb-columnstore-api-python3_1.2.*_amd64.deb</td><td>The Python part needed to use the API in Python 3</td></tr>
<tr><td>mariadb-columnstore-api-spark_1.2.*_amd64.deb</td><td>The Scala/Spark part including ColumnStoreExporter</td></tr>
<tr><td>mariadb-columnstore-api-pyspark_1.2.*_amd64.deb</td><td>The Python 2/Spark part including ColumnStoreExporter</td></tr>
<tr><td>mariadb-columnstore-api-pyspark3_1.2.*_amd64.deb</td><td>The Python 3/Spark part including ColumnStoreExporter</td></tr>
</tbody></table>

### Debian 8 Package

The following libraries need to be installed on the system for the package install (jessie-backports is needed to install libuv1):

```sql
echo "deb http://httpredir.debian.org/debian jessie-backports main contrib non-free" >> /etc/apt/sources.list
apt-get update
apt-get install libuv1 libxml2 libsnappy1
```

The API deb package can be installed:

```sql
dpkg -i mariadb-columnstore-api*.deb
```

In addition installing OpenJDK8 for java support requires the backports repo and the following command:

```sql
apt-get install -t jessie-backports openjdk-8-jdk
```

#### Bulk Write SDK 1.2.2

Starting with version 1.2.2 the bulk write SDK is split up into its components which can be installed separately. Packages are:

<table><tbody><tr><th>package name</th><th>description</th></tr>
<tr><td>maraidb-columnstore-api-cpp_1.2.*_amd64.deb</td><td>The base C++ part needed to execute programs</td></tr>
<tr><td>mariadb-columnstore-api-cpp-devel_1.2.*_amd64.deb</td><td>The C++ development part needed to build programs</td></tr>
<tr><td>mariadb-columnstore-api-java_1.2.*_amd64.deb</td><td>The Java part needed to use the API in Java</td></tr>
<tr><td>mariadb-columnstore-api-python_1.2.*_amd64.deb</td><td>The Python part needed to use the API in Python 2</td></tr>
<tr><td>mariadb-columnstore-api-python3_1.2.*_amd64.deb</td><td>The Python part needed to use the API in Python 3</td></tr>
<tr><td>mariadb-columnstore-api-spark_1.2.*_amd64.deb</td><td>The Scala/Spark part including ColumnStoreExporter</td></tr>
<tr><td>mariadb-columnstore-api-pyspark_1.2.*_amd64.deb</td><td>The Python 2/Spark part including ColumnStoreExporter</td></tr>
<tr><td>mariadb-columnstore-api-pyspark3_1.2.*_amd64.deb</td><td>The Python 3/Spark part including ColumnStoreExporter</td></tr>
</tbody></table>

### Windows 10 Package

To install the SDK on Windows you simply have to follow the installation wizard of the installer. On Windows, the SDK requires the [Visual Studio 2015/2017 C++ Redistributable (x64)](https://www.microsoft.com/en-us/download/details.aspx?id=48145) to operate. It will be installed automatically if not detected. If Python 2.7 or Python 3.7 is detected during the installation of the SDK, pymcsapi will be installed directly into the regarding Python installation.

## Environment Configuration

If the SDK is being installed to a server that is not part of a MariaDB ColumnStore server then it requires a local copy of the ColumnStore.xml file in order to determine how to connect to ColumnStore. The simplest approach is to copy this file from one of the ColumnStore servers to one of the following 2 locations on the SDK server ensuring read privileges for the OS user being used:

- /usr/local/mariadb/columnstore/etc/Columnstore.xml
- $COLUMNSTORE_INSTALL_DIR/etc/Columnstore.xml

Alternatively, a custom file location may be passed as an argument to the ColumnStoreDriver constructor. This is also necessary if you plan to write to multiple ColumnStore servers from the same host. The SDK server must be able to communicate to the ColumnStore PM servers over the standard ColumnStore TCP ports 8616, 8630, and 8800.

If the ColumnStore server was configured as a single server deployment, then the Columnstore.xml file will need the IP addresses updated from 127.0.0.1 to the actual ip / hostname of the ColumnStore server in order to be used on a remote SDK server. A simple sed statement should suffice for updating:

```sql
sed "s/127.0.0.1/172.21.21.8/g" Columnstore.xml > Columnstore_new.xml
```

## Getting Started with C++

The documentation below is the best place to get started with building and developing against the C++ SDK. Some sample programs are installed to /usr/share/doc/mcsapi/ for review.

## Getting Started with Java

The Java version of the SDK provides a very similar API to the C++ one so the pdf documentation can generally be transposed 1 for 1 to understand the API calls. A dedicated Java usage documentation was added with version 1.1.6.

Since the Java version is a wrapper on top of the C++ API the underlying library must be loaded using a static initializer once in your program.

Starting with version 1.1.3 the library is loaded whilst importing the ColumnStoreDriver through:

```sql
import com.mariadb.columnstore.api.ColumnStoreDriver;
```

Versions prior to 1.1.3 need to manually load the system library:

```sql
    static {
        System.loadLibrary("javamcsapi"); // use _javamcsapi for centos7
    }
```

The corresponding java jar must be also be included in the java classpath. The packaged install is built and tested with OpenJDK 8.

First a simple table is created with the mcsmysql client:

```sql
MariaDB [test]> create table t1(i int, c char(3)) engine=columnstore;
```

Next create a file MCSAPITest.java with the following contents:

```sql
import com.mariadb.columnstore.api.*;

public class MCSAPITest {
    
        public static void main(String[] args) {
        ColumnStoreDriver d = new ColumnStoreDriver();
        ColumnStoreBulkInsert b = d.createBulkInsert("test", "t1", (short)0, 0);
        try {
            b.setColumn(0, 2);
            b.setColumn(1, "XYZ");
            b.writeRow();
            b.commit();
        }
        catch (ColumnStoreException e) {
            b.rollback();
            e.printStackTrace();
        }
    }
}
```

Now compile and run the program. For RHEL / CentOS 7 (library installed in /usr/lib64):

```sql
javac -classpath ".:/usr/lib64/javamcsapi.jar" MCSAPITest.java
java -classpath ".:/usr/lib64/javamcsapi.jar" MCSAPITest
```

For Ubuntu / Debian:

```sql
javac -classpath ".:/usr/lib/javamcsapi.jar" MCSAPITest.java
java -classpath ".:/usr/lib/javamcsapi.jar" MCSAPITest
```

Now back in mcsmysql verify the data is written:

```sql
MariaDB [test]> select * from t1;
+------+------+
| i    | c    |
+------+------+
|    2 | XYZ  |
+------+------+
```

## Getting Started with Python

The current package install supports Python 2.7 and Python 3. Once installed the library is available for immediate use on the system. A dedicated Python usage documentation was added with version 1.1.6.

First a simple table is created with the mcsmysql client:

```sql
MariaDB [test]> create table t1(i int, c char(3)) engine=columnstore;
```

For this simple test the python CLI will be used by simply running the python program with no arguments and entering the following:

```sql
import pymcsapi
driver = pymcsapi.ColumnStoreDriver()
bulk = driver.createBulkInsert('test', 't1', 0, 0)
bulk.setColumn(0,1)
bulk.setColumn(1, 'ABC')
bulk.writeRow()
bulk.commit()
```

In interactive command line mode, the bulk.setColumn and bulk.writeRow methods return the bulk object to allow for more concise chained invocation. You may see something like the following as a result which is normal:

```sql
>>> bulk.setColumn(0,1)
<pymcsapi.ColumnStoreBulkInsert; proxy of <Swig Object of type 'mcsapi::ColumnStoreBulkInsert *' at 0x7f0d5295bcc0> >
```

Now back in mcsmysql verify the data is written:

```sql
MariaDB [test]> select * from t1;
+------+------+
| i    | c    |
+------+------+
|    1 | ABC  |
+------+------+
```

# Documentation

The following documents provide SDK documentation:

- Usage documentation for C++ ([PDF](/kb/en/columnstore-bulk-write-sdk/+attachment/libmcsapi_usage_1_2_3), [HTML](/kb/en/columnstore-bulk-write-sdk/+attachment/libmcsapi_usage_html_1_2_3)), Python ([PDF](/kb/en/columnstore-bulk-write-sdk/+attachment/pymcsapi_usage_1_2_3), [HTML](/kb/en/columnstore-bulk-write-sdk/+attachment/pymcsapi_usage_html_1_2_3)), and Java ([PDF](/kb/en/columnstore-bulk-write-sdk/+attachment/javamcsapi_usage_1_2_3), [HTML](/kb/en/columnstore-bulk-write-sdk/+attachment/javamcsapi_usage_html_1_2_3)) for 1.2.3 GA
- Usage documentation for C++ ([PDF](/kb/en/columnstore-bulk-write-sdk/+attachment/libmcsapi_usage_1_2_2), [HTML](/kb/en/columnstore-bulk-write-sdk/+attachment/libmcsapi_usage_html_1_2_2)), Python ([PDF](/kb/en/columnstore-bulk-write-sdk/+attachment/pymcsapi_usage_1_2_2), [HTML](/kb/en/columnstore-bulk-write-sdk/+attachment/pymcsapi_usage_html_1_2_2)), and Java ([PDF](/kb/en/columnstore-bulk-write-sdk/+attachment/javamcsapi_usage_1_2_2), [HTML](/kb/en/columnstore-bulk-write-sdk/+attachment/javamcsapi_usage_html_1_2_2)) for 1.2.2 GA
- Usage documentation for [C++](/kb/en/columnstore-bulk-write-sdk/+attachment/libmcsapi_usage_1_2_1), [Python](/kb/en/columnstore-bulk-write-sdk/+attachment/pymcsapi_usage_1_2_1) and  [Java](/kb/en/columnstore-bulk-write-sdk/+attachment/javamcsapi_usage_1_2_1) for 1.2.1 Beta
- Usage documentation for [C++](/kb/en/columnstore-bulk-write-sdk/+attachment/libmcsapi_usage_1_2_0), [Python](/kb/en/columnstore-bulk-write-sdk/+attachment/pymcsapi_usage_1_2_0) and  [Java](/kb/en/columnstore-bulk-write-sdk/+attachment/javamcsapi_usage_1_2_0) for 1.2.0 Alpha
- Usage documentation for [C++](/kb/en/columnstore-bulk-write-sdk/+attachment/libmcsapi_usage_1_1_6), [Python](/kb/en/columnstore-bulk-write-sdk/+attachment/pymcsapi_usage_1_1_6) and  [Java](/kb/en/columnstore-bulk-write-sdk/+attachment/javamcsapi_usage_1_1_6) for 1.1.6 GA
- [Building](/kb/en/columnstore-bulk-write-sdk/+attachment/libmcsapi_building_1_1_4), [Usage](/kb/en/columnstore-bulk-write-sdk/+attachment/libmcsapi_usage_1_1_4) for 1.1.4 GA
- [Building](/kb/en/columnstore-bulk-write-sdk/+attachment/libmcsapi_building_1_1_3), [Usage](/kb/en/columnstore-bulk-write-sdk/+attachment/libmcsapi_usage_1_1_3) for 1.1.3 GA
- [Building](/kb/en/columnstore-bulk-write-sdk/+attachment/libmcsapi_building_1_1_2), [Usage](/kb/en/columnstore-bulk-write-sdk/+attachment/libmcsapi_usage_1_1_2) for 1.1.2 GA
- [Building](/kb/en/columnstore-bulk-write-sdk/+attachment/libmcsapi_building_1_1_1), [Usage](/kb/en/columnstore-bulk-write-sdk/+attachment/libmcsapi_usage_1_1_1) for 1.1.1 RC
- [Building](/kb/en/columnstore-bulk-write-sdk/+attachment/libmcsapi_building_1), [Usage](/kb/en/columnstore-bulk-write-sdk/+attachment/libmcsapi_usage_1) for 1.1.0 Beta