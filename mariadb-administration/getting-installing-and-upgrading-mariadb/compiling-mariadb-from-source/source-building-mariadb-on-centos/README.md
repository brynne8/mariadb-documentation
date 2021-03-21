# Building MariaDB on CentOS

In the event that you are using the Linux-based operating system CentOS or any of its derivatives, you can optionally compile MariaDB from source code.  This is useful in cases where you want use a more advanced release than the one that's available in the official repositories, or when you want to enable certain feature that are not otherwise accessible.

## Installing Build Dependencies

Before you start building MariaDB, you first need to install the build dependencies required to run the compile.  CentOS provides a tool for installing build dependencies.  The `yum-builddep` utility reads a package and generates a list of the packages required to build from source, then calls YUM to install them for you.  In the event that this utility is not available on your system, you can install it through the `yum-utils` package.  Once you have it, install the MariaDB build dependencies.

```sql
# yum-builddep mariadb-server
```

Running this command installs many of the build dependencies, but it doesn't install all of them.  Not all the required packages are noted and it's run against the official CentOS package of MariaDB, not necessarily the version that you want to install.  Use YUM to install the remaining packages.

```sql
# yum install git \
      gcc \
      gcc-c++ \
      bison \
      libxml2-devel \
      libevent-devel \
      rpm-build
```

In addition to these, you also need to install `gnutls` or `openssl`, depending on the TLS implementation you want to use.

For more information on dependencies, see [Linux Build Environment](/mariadb-administration/getting-installing-and-upgrading-mariadb/compiling-mariadb-from-source/Build_Environment_Setup_for_Linux/).

## Building MariaDB

Once you have the base dependencies installed, you can retrieve the source code and start building MariaDB.  The source code is available on GitHub.  Use the `--branch` option to specify the particular version of MariaDB you want to build.

```sql
$ git clone --branch 10.3 https://github.com/MariaDB/server.git
```

With the source repository cloned onto your system, you can start building MariaDB.  Run CMake to read MariaDB for the build,

```sql
$ cmake -DRPM=centos7 server/
```

Once CMake readies the relevant Makefile for your system, use Make to build MariaDB.

```sql
$ make package
```

This generates an RPM file, which you can then install on your system or copy over to install on other CentOS hosts.

## Creating MariaDB-compat package

MariaDB-compat package contains libraries from older MariaDB releases. They cannot be built from the current source tree, so cpack creates them by repackaging old MariaDB-shared packages. If you want to have -compat package created, you need to download MariaDB-shared-5.3 and MariaDB-shared-10.1 rpm packages for your architecture (any minor version will do) and put them <em>one level above</em> the source tree you're building. CMake will pick them up and create a MariaDB-compat package. CMake reports it as

```sql
$ ls ../*.rpm
../MariaDB-shared-10.1.17-centos7-x86_64.rpm
../MariaDB-shared-5.3.12-122.el5.x86_64.rpm
$ cmake -DRPM=centos7 .
...
Using ../MariaDB-shared-5.3.12-122.el5.x86_64.rpm to build MariaDB-compat
Using ../MariaDB-shared-10.1.17-centos7-x86_64.rpm to build MariaDB-compat
```

## Additional Dependencies

In the event that you miss a package while installing build dependencies, CMake may continue to fail after you install the necessary packages.  If this happens to you, delete the CMake cache then run the above the command again:

```sql
$ rm CMakeCache.txt
```

When CMake runs through the tests again, it should now find the packages it needs, instead of the cache telling it they're unavailable.