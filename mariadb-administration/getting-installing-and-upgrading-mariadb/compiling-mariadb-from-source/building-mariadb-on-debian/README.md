# Building MariaDB on Debian

In the event that you are using the Linux-based operating system Debian or any of its direct derivatives and would like to compile MariaDB from source code, you can do so using the MariaDB source repository for the release that interests you.  For Ubuntu and its derivatives, see [Building on Ubuntu](/mariadb-administration/getting-installing-and-upgrading-mariadb/compiling-mariadb-from-source/building-mariadb-on-ubuntu/).

Before you begin, install the `software-properties-common` and `devscripts` packages:

```sql
$ sudo apt-get install software-properties-common \
      devscripts
```

## Installing Build Dependencies

MariaDB requires a number of packages to compile from source.  Fortunately, you can use the MariaDB repositories to retrieve the necessary code for the version you want.  Use the [Repository Configuration](https://downloads.mariadb.org/mariadb/repositories/) tool to determine how to set up the MariaDB repository for your release of Debian, the version of MariaDB that you want to install, and the mirror that you want to use.

First add the authentication key for the repository, then add the repository.

```sql
$ sudo apt-key adv --recv-keys \
      --keyserver hkp://keyserver.ubuntu.com:80 \
      0xF1656F24C74CD1D8
$ sudo add-apt-repository 'deb [arch=amd64] http://nyc2.mirrors.digitalocean.com/mariadb/repo/10.3/debian stretch main'
```

The second command added text to the `/etc/apt/sources.list` file.  One of these lines is the repository containing binary packages for MariaDB, the other contains the source packages.  The line for the source packages is commented out by default.  Open the file using your preferred text editor and uncomment the source repository.

```sql
$ sudo vim /etc/apt/sources.list

...
## This software is not part of Ubuntu, but is offered by Canonical and the
deb [arch=amd64] http://nyc2.mirrors.digitalocean.com/mariadb/repo/10.3/debian stretch main
deb-src [arch=amd64] http://nyc2.mirrors.digitalocean.com/mariadb/repo/10.3/debian stretch main
```

Once the repository is set up, you can use `apt-get` to retrieve the build dependencies.  MariaDB packages supplied by Ubuntu and packages supplied by the MariaDB repository have the same base name of `mariadb-server`.  You need to specify the specific version you want to retrieve.

```sql
$ apt-get build-dep mariadb-server-10.3
```

## Building MariaDB

Once you have the base dependencies installed, you can retrieve the source code and start building MariaDB.  The source code is available on GitHub.  Use the `--branch` option to specify the particular version of MariaDB you want to build.

```sql
$ git clone --branch 10.3 https://github.com/MariaDB/server.git
```

The source code includes scripts to install the remaining build dependencies.  For Ubuntu, they're located in the `debian/` directory.  Navigate into the repository and run the `autobase-deb.sh` script.  Then use

```sql
$ cd server/
$ ./debian/autobake-deb.sh
```

Using the `mk-build-deb` and `apt-get`, install the remaining build dependencies

```sql
$ mk-build-deb
$ apt-get install ./mariadb-*build-deps_*.deb
```

Lastly, call the `autobake-deb.sh` script again to build MariaDB.

```sql
$ ./debian/autobake-deb.sh
```

### After Building

After building the packages, it is a good idea to put them in a repository. See the [Creating a Debian Repository](/mariadb-administration/getting-installing-and-upgrading-mariadb/compiling-mariadb-from-source/Creating_a_Debian_Repository/) page for instructions.