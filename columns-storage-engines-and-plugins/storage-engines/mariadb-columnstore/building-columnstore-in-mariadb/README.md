# Building ColumnStore in MariaDB

This is a description of how to build and start a local ColumnStore installation, for debugging purposes.

## Install the dependencies

For CentOS:

```sql
yum -y groupinstall "Development Tools" \
   && yum -y install bison ncurses-devel readline-devel perl-devel openssl-devel cmake libxml2-devel gperf libaio-devel libevent-devel python-devel ruby-devel tree wget pam-devel snappy-devel libicu \
   && yum -y install vim wget strace ltrace gdb  rsyslog net-tools openssh-server expect \
   && boost perl-DBI
```

## Get the source code

```sql
git clone https://github.com/mariadb-corporation/mariadb-columnstore-server.git
cd mariadb-columnstore-server/
git clone https://github.com/mariadb-corporation/mariadb-columnstore-engine.git
```

## Compile

```sql
cmake . -DCMAKE_BUILD_TYPE=Debug \
  -DWITHOUT_MROONGA:bool=1 -DWITHOUT_TOKUDB:bool=1 \
  -DCMAKE_INSTALL_PREFIX=/usr/local/mariadb/columnstore/mysql
make -j10
sudo make install
```

```sql
cd mariadb-columnstore-engine/
cmake . -DCMAKE_BUILD_TYPE=Debug
make -j10
sudo make install
cd /usr/local/mariadb/columnstore/bin/
```

## Configure

Make sure you do NOT have `/etc/my.cnf` or `/.my.cnf`.

```sql
sudo ./postConfigure
```

Answer "Enter" to all questions, except:

```sql
Select the type of System Server install [1=single, 2=multi] (2) > 
```

Here, answer `1`.

Access the server

```sql
source /usr/local/mariadb/columnstore/bin/columnstoreAlias
mcsmysql
```