# Get, Build and Test Latest MariaDB the Lazy Way

The intention of this documentation is show all the steps of getting, building and testing the latest MariaDB server (10.5 at time of writing) from GitHub.  Each stage links to the full documentation for that step if you need to find out more.

## [Install all tools needed to build MariaDB](/mariadb-administration/getting-installing-and-upgrading-mariadb/compiling-mariadb-from-source/Build_Environment_Setup_for_Linux/)

### OpenSuse

```sql
sudo zypper install git gcc gcc-c++ make bison ncurses ncurses-devel zlib-devel libevent-devel cmake openssl
```

### Debian

```sql
apt install -y build-essential bison
apt build-dep mariadb-server 
```

## [Set Up git](/kb/en/using-git/)

Fetch and checkout the MariaDB source to a subdirectory of the current directory

```sql
git clone https://github.com/MariaDB/server.git mariadb
cd mariadb
git checkout 10.5
```

## [Build It](/mariadb-administration/getting-installing-and-upgrading-mariadb/compiling-mariadb-from-source/generic-build-instructions/)

The following command builds a server the same way that is used for building releases. Use `cmake . -DCMAKE_BUILD_TYPE=Debug` to build for debugging.

```sql
cmake . -DBUILD_CONFIG=mysql_release && make -j8
```

## [Check the Server (If You Want To)](%5Bmysql-test)

```sql
cd mysql-test
mtr --parallel=8 --force
```

## [Install the Default Databases](/clients-utilities/mariadb-install-db/)

```sql
./scripts/mariadb-install-db --srcdir=.
```

(Older MariaDB version use [mysql_install_db](/clients-utilities/mysql_install_db/))

## Install the Server (If needed)

You can also [run and test mariadb directly from the build directory](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/running-mariadb-from-the-build-directory/), in which case you can skip the rest of the steps below.

```sql
make install
```

### [Start the Server](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/starting-and-stopping-mariadb-automatically/)

Start the server in it's own terminal window for testing. Note that the directory depends on your system!

```sql
/usr/sbin/mysqld
```