# Building the Galera wsrep Package on Ubuntu and Debian

The instructions on this page were used to create the <em>galera</em> package on the Ubuntu and Debian  Linux distributions. This package contains the wsrep provider for [MariaDB Galera Cluster](/kb/en/galera/).

##### MariaDB Galera Cluster starting with 5.5.35

Starting with MariaDB Galera Cluster 5.5.35, the version of the wsrep provider is <strong>25.3.5</strong>. We also provide <strong>25.2.9</strong> for those that need or want it.

Prior to that, the wsrep version was 23.2.7.

1 Install prerequisites:

```sql
sudo apt-get update
sudo apt-get upgrade
sudo apt-get -y install check debhelper libasio-dev libboost-dev libboost-program-options-dev libssl-dev scons
```

1 Clone [galera.git](https://github.com/mariadb/galera) from [github.com/mariadb](https://github.com/mariadb) and checkout mariadb-3.x banch:

```sql
git init repo
cd repo
git clone -b mariadb-3.x https://github.com/MariaDB/galera.git
```

1 Build the packages by executing `build.sh` under scripts/ directory with `-p` switch:

```sql
cd galera
./scripts/build.sh -p
```

When finished, you will have the Debian packages for galera library and arbitrator in the parent directory.

## Running galera test suite

If you want to run the `galera` test suite (`mysql-test-run --suite=galera`), you need to install the galera library as either `/usr/lib/galera/libgalera_smm.so` or `/usr/lib64/galera/libgalera_smm.so`