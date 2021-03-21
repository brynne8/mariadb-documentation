# Installing Galera from Source

There are binary installation packages available for RPM and Debian-based distributions, which will pull in all required Galera dependencies.

If these are not available, you will need to build Galera from source.

##### MariaDB starting with [10.1](/kb/en/what-is-mariadb-101/)

Starting from [MariaDB 10.1](/kb/en/what-is-mariadb-101/), the wsrep API for Galera Cluster is included by default. Follow 
the usual [compiling-mariadb-from-source](/mariadb-administration/getting-installing-and-upgrading-mariadb/compiling-mariadb-from-source) instructions

##### MariaDB until [10.0](/kb/en/what-is-mariadb-100/)

[MariaDB 10.0](/kb/en/what-is-mariadb-100/) and below are no longer supported. The instructions below have only historical significance.

## Preparation

<em>make</em> cannot manage  dependencies for the build process, so the following packages need to be installed first:

RPM-based:

```sql
yum-builddep MariaDB-server
```

Debian-based:

```sql
apt-get build-dep mariadb-server
```

If running on an alternative system, or the commands are available, the following packages are required. You will need to check the repositories for the correct package names on your distribution - these may differ between distributions, or require additional packages:

#### MariaDB Database Server with wsrep API

- Git, CMake (on Fedora, both cmake and cmake-fedora are required), GCC and GCC-C++, Automake, Autoconf, and Bison, as well as development releases of libaio and ncurses.

## Building

You can use Git to download the source code, as MariaDB source code is available through GitHub.
Clone the repository:

```sql
git clone https://github.com/mariadb/server mariadb
```

1 Checkout the branch (e.g. 10.0-galera or 5.5-galera), for example:

```sql
cd mariadb
git checkout 10.0-galera
```

### Building the Database Server

The standard and Galera cluster database servers are the same, except that for Galera Cluster, the wsrep API patch is included. Enable the patch with the  CMake configuration options `WITH_WSREP` and `WITH_INNODB_DISALLOW_WRITES`. To build the database server, run the following commands:

```sql
cmake -DWITH_WSREP=ON -DWITH_INNODB_DISALLOW_WRITES=ON .
make
make install
```

There are also some build scripts in the <em>BUILD/ </em>directory which may be more convenient to use. For example, the following pre-configures the build options discussed above:

```sql
./BUILD/compile-pentium64-wsrep
```

There are several others as well, so you can select the most convenient.

Besides the server with the Galera support, you will also need a galera provider.

## Preparation

<em>make</em> cannot manage  dependencies itself, so the following packages need to be installed first:

```sql
apt-get install -y scons check 
```

If running on an alternative system, or the commands are available, the following packages are required. You will need to check the repositories for the correct package names on your distribution - these may differ between distributions, or require additional packages:

#### Galera Replication Plugin

- SCons, as well as development releases of Boost (libboost_program_options, libboost_headers1), Check and OpenSSL.

## Building

Run:

```sql
git clone -b mariadb-4.x https://github.com/MariaDB/galera.git
```

If you are using [MariaDB 10.3](/kb/en/what-is-mariadb-103/) or earlier, you should checkout `mariadb-3.x` instead.

After this, the source files for the Galera provider will be in the `galera` directory.

### Building the Galera Provider

The Galera Replication Plugin both implements the wsrep API and operates as the database server's wsrep Provider. To build, cd into the <em>galera/ </em> directory and do:

```sql
git submodule init
git submodule update
./scripts/build.sh
mkdir /usr/lib64/galera
cp libgalera_smm.so /usr/lib64/galera
```

The path to `libgalera_smm.so` needs to be defined in the <em>my.cnf</em> configuration file.

Building Galera Replication Plugin from source on FreeBSD runs into issues due to Linux dependencies. To overcome these, either install the binary package: `pkg install galera`, or use the ports build available at `/usr/ports/databases/galera`.

## Configuration

After building, a number of other steps are necessary:

- Create the database server user and group:

```sql
 groupadd mysql
 useradd -g mysql mysql
```

- Install the database (the path may be different if you specified CMAKE_INSTALL_PREFIX):

```sql
 cd /usr/local/mysql
 ./scripts/mysql_install_db --user=mysql
```

- If you want to install the database in a location other than <em> /usr/local/mysql/data </em>, use the <em>--basedir</em> or <em>--datadir</em> options.
- Change the user and group permissions for the base directory.

```sql
 chown -R mysql /usr/local/mysql
 chgrp -R mysql /usr/local/mysql
```

- Create a system unit for the database server.

```sql
cp /usr/local/mysql/supported-files/mysql.server /etc/init.d/mysql
chmod +x /etc/init.d/mysql
chkconfig --add mysql
```

- Galera Cluster can now be started using the service command, and is set to start at boot.