# Oracle XE 11.2. and MariaDB 10.1 integration on Ubuntu 14.04 and Debian systems

1) Sign up for Oracle downloads and download Oracle Express at:

[http://www.oracle.com/technetwork/database/database-technologies/express-edition/downloads/index.html](http://www.oracle.com/technetwork/database/database-technologies/express-edition/downloads/index.html)

- Sign up (unless already) and log in.
- Accept the license agreement.
- Download Oracle Database Express Edition 11g Release 2 for Linux x64
(oracle-xe-11.2.0-1.0.x86_64.rpm.zip, version numbers may change over time)

2) Prepare apt-get on your system

```sql
sudo add-apt-repository ppa:webupd8team/java
<press Enter to accept>
sudo apt-get update
sudo apt-get install oracle-java8-installer
```

3) After Java installation, verify the version

```sql
java -version
java version "1.8.0_121"
```

4) Edit /etc/bash.bashrc

Scroll to the bottom of the file and add the following lines.

```sql
export JAVA_HOME=/usr/lib/jvm/java-8-oracle
export PATH=$JAVA_HOME/bin:$PATH
```

Save and check:

```sql
source /etc/bash.bashrc
echo $JAVA_HOME
/usr/lib/jvm/java-8-oracle
```

5) Additional packages are required, unless installed already. Run the command:

```sql
sudo apt-get install alien libaio1 unixodbc
```

6)

```sql
unzip oracle-xe-11.2.0-1.0.x86_64.rpm.zip
cd Disk1/
sudo alien --scripts -d oracle-xe-11.2.0-1.0.x86_64.rpm
```

This step might take some time. You may proceed steps 7)-11)
in the meanwhile in another terminal window.

7)

Create a new file /sbin/chkconfig and add the following contents

```sql
#!/bin/bash
# Oracle 11gR2 XE installer chkconfig for Ubuntu
file=/etc/init.d/oracle-xe
if [[ ! `tail -n1 $file | grep INIT` ]]; then
echo >> $file
echo '### BEGIN INIT INFO' >> $file
echo '# Provides: OracleXE' >> $file
echo '# Required-Start: $remote_fs $syslog' >> $file
echo '# Required-Stop: $remote_fs $syslog' >> $file
echo '# Default-Start: 2 3 4 5' >> $file
echo '# Default-Stop: 0 1 6' >> $file
echo '# Short-Description: Oracle 11g Express Edition' >> $file
echo '### END INIT INFO' >> $file
fi
update-rc.d oracle-xe defaults 80 01
#EOF
```

8)

```sql
sudo chmod 755 /sbin/chkconfig
```

9)

Create a new file /etc/sysctl.d/60-oracle.conf

Copy and paste the following into the file. Kernel.shmmax is the
maximum possible value of physical RAM in bytes. 536870912 / 1024
/1024 = 512 MB.

```sql
#Oracle 11g XE kernel parameters  
fs.file-max=6815744  
net.ipv4.ip_local_port_range=9000 65000  
kernel.sem=250 32000 100 128 
kernel.shmmax=536870912
```

10)

```sql
sudo service procps start
sudo sysctl -q fs.file-max
```

1 This method should return the following:
fs.file-max = 6815744

11) Some additional steps required

```sql
sudo ln -s /usr/bin/awk /bin/awk
mkdir /var/lock/subsys
touch /var/lock/subsys/listener
```

12) Install Oracle XE (should have been converted from .rpm to .deb by now)

```sql
sudo dpkg --install oracle-xe_11.2.0-2_amd64.deb
```

13)

Execute the  following  to avoid getting  a ORA-00845: MEMORY_TARGET
error. Note: replace "size=4096m" with the size of your (virtual)
machine RAM in MBs.

```sql
sudo rm -rf /dev/shm
sudo mkdir /dev/shm
sudo mount -t tmpfs shmfs -o size=4096m /dev/shm
```

14) Create the file /etc/rc2.d/S01shm_load

NOTE: replace "size=4096m"
with the size of your machine RAM in MBS.

```sql
#!/bin/sh
case "$1" in
start) mkdir /var/lock/subsys 2>/dev/null
touch /var/lock/subsys/listener
rm /dev/shm 2>/dev/null
mkdir /dev/shm 2>/dev/null
mount -t tmpfs shmfs -o size=4096m /dev/shm ;;
*) echo error
exit 1 ;;
esac
```

```sql
sudo chmod 755 /etc/rc2.d/S01shm_load
```

15) Configure Oracle 11g R2 Express Edition. Default answers are probably OK.

```sql
sudo /etc/init.d/oracle-xe configure
```

Specify the HTTP port that will be used for Oracle Application Express [8080]:

Specify a port that will be used for the database listener [1521]:

Specify a password to be used for database accounts.  Note that the same
password will be used for SYS and SYSTEM.  Oracle recommends the use of 
different passwords for each database account.  This can be done after 
initial configuration:

Do you want Oracle Database 11g Express Edition to be started on boot (y/n) [y]:

16) Edit /etc/bash.bashrc. Add to the end of the file:

```sql
export ORACLE_HOME=/u01/app/oracle/product/11.2.0/xe
export ORACLE_SID=XE
export NLS_LANG=`$ORACLE_HOME/bin/nls_lang.sh`
export ORACLE_BASE=/u01/app/oracle
export LD_LIBRARY_PATH=$ORACLE_HOME/lib:$LD_LIBRARY_PATH
export PATH=$ORACLE_HOME/bin:$PATH
```

Save.

17) Run source command and check that the output makes sense

```sql
source /etc/bash.bashrc
echo $ORACLE_HOME
/u01/app/oracle/product/11.2.0/xe
```

18) Apply desktop icon changes and start Oracle:

```sql
sudo chmod a+x ~/Desktop/oraclexe-gettingstarted.desktop
sudo service oracle-xe start
```

19) Download SQL Developer package

Download Oracle SQL Developer from the Oracle site. Select the Linux
RPM package:
[http://www.oracle.com/technetwork/developer-tools/sql-developer/downloads/index.html](http://www.oracle.com/technetwork/developer-tools/sql-developer/downloads/index.html)

Choose the Linux RPM package.

20) Install SQL Developer

```sql
sudo alien --scripts -d sqldeveloper-4.1.5.21.78-1.noarch.rpm
sudo dpkg --install sqldeveloper_4.1.5.21.78-2_all.deb
mkdir ~/.sqldeveloper
```

21) Run sqldeveloper:

```sql
sudo /opt/sqldeveloper/sqldeveloper.sh
```

1 Tell sqldeveloper the correct Java path, if it asks for it:

/usr/lib/jvm/java-8-oracle

```sql
- Click connections
- Add new connection
- Connection name: XE
- username: SYSTEM
- password: <your-password>

- Connection type: Basic Role: Default
- Hostname: localhost
- Port: 1521
- SID: xe
```

## MariaDB

22) Install MariaDB

[https://downloads.mariadb.org/mariadb/repositories/](https://downloads.mariadb.org/mariadb/repositories/)
Instructions are for Ubuntu, but choose the one that is appropriate:
- Ubuntu
- 14.04 LTS "trusty"
- 10.1 [Stable]
- choose a mirror

Run the commands given for you. For example (DO NOT COPY PASTE BELOW, CHECK WHAT THE MariaDB PAGE TELLS YOU TO DO):

```sql
sudo apt-get install software-properties-common
sudo apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xcbcb082a1bb943db
sudo add-apt-repository 'deb [arch=amd64,i386,ppc64el] http://mirror.netinch.com/pub/mariadb/repo/10.1/ubuntu trusty main'
sudo apt-get update
```

1 Install mariadb-server:

```sql
sudo apt-get install mariadb-server
```

23) Install ODBC driver

[https://downloads.mariadb.org/connector-odbc/](https://downloads.mariadb.org/connector-odbc/)

1 MariaDB Connector/ODBC 2.0.13 Stable for Linux

Download: mariadb-connector-[ODBC-2](https://jira.mariadb.org/browse/ODBC-2).0.13-ga-linux-x86_64.tar.gz

```sql
tar xfz mariadb-connector-odbc-2.0.13-ga-linux-x86_64.tar.gz
sudo cp -p mariadb-connector-odbc-2.0.13-ga-linux-x86_64/lib/libmaodbc.so /lib
sudo ldconfig
```

24) Install unixodbc and mariadb-connect engine

```sql
apt-get install unixodbc-dev
apt-get install unixodbc-bin
apt-get install unixodbc
apt-get install libodbc1
apt-get install mariadb-connect-engine-10.1
```

25) Edit /etc/odbcinst.ini

Add:

```sql
[Oracle ODBC driver for Oracle 11.2]
Description     = Oracle 11.2 ODBC driver
Driver          = /u01/app/oracle/product/11.2.0/xe/lib/libsqora.so.11.1
```

26) Edit /etc/odbc.ini

Add (check your password):

```sql
[XE]
Driver          = Oracle ODBC driver for Oracle 11.2
ServerName      = //localhost:1521/xe
DSN             = XE
UserName        = SYSTEM
Password        = <your-password>
```

27) Test ODBC connection and add a table

```sql
isql -v XE SYSTEM <your-password>

create table t1 (i int);
insert into t1 (i) values (1);
insert into t1 (i) values (3);
insert into t1 (i) values (5);
insert into t1 (i) values (8);
select i from t1;
```

And you should see the rows. You can test the
same with sqldeveloper, open XE connection and run
select i from t1; in Worksheet.

28) Edit /etc/init.d/mysql

Add:

```sql
export JAVA_HOME=/usr/lib/jvm/java-8-oracle 
export PATH=$JAVA_HOME/bin:$PATH 
export ORACLE_HOME=/u01/app/oracle/product/11.2.0/xe 
export CLIENT_HOME=$ORACLE_HOME 
export ORACLE_SID=XE 
export NLS_LANG=`$ORACLE_HOME/bin/nls_lang.sh` 
export ORACLE_BASE=/u01/app/oracle 
export LD_LIBRARY_PATH=$ORACLE_HOME/lib:$LD_LIBRARY_PATH 
export PATH=$ORACLE_HOME/bin:$PATH 
```

Right after END INIT INFO. Otherwise mysqld will not find the Oracle ODBC driver.

28) Restart MariaDB

```sql
sudo /etc/init.d/mysql restart
```

29)

```sql
mysql -uroot -p

CREATE DATABASE mdb;
USE mdb;
INSTALL SONAME 'ha_connect';
CREATE TABLE t1 ENGINE=CONNECT TABLE_TYPE=ODBC tabname='T1' CONNECTION='DSN=XE;UID=SYSTEM;PWD=<your-password>';

select I from t1;
```

You should see the previously inserted values 1,3,5 and 8.
Using isql or sqldeveloper, add another rows with values 9 and 11.
Remember to commit, if you are using Oracle sqldeveloper. You should now see
the added values via MariaDB client (mysql-client) connection.

To be examined: inserting values to the t1 table from mariadb connection
does not work. It gives a precision error from Oracle side.

## Connect to MariaDB via JDBC

Download MySQL Connector from
[https://dev.mysql.com/downloads/connector/j/5.0.html](https://dev.mysql.com/downloads/connector/j/5.0.html)

Select Version (for example 5.0.8)

Platform independent. Download mysql-connector-java-5.0.8.tar.gz

```sql
tar xvfz mysql-connector-java-5.0.8.tar.gz
cd mysql-connector-java-5.0.8/
sudo cp -p mysql-connector-java-5.0.8-bin.jar /usr/lib/jvm/java-8-oracle/lib/mariadb/

sudo /opt/sqldeveloper/sqldeveloper.sh
```

In SQL Developer choose Tools -&gt; Preferences

Expand the "Database" option in the left hand tree

Click on "Third Party JDBC Drivers"

Click on "Add Entry..."

Navigate to your third-party driver jar file and choose OK

/usr/lib/jvm/java-8-oracle/lib/mariadb/mysql-connector-java-5.0.8-bin.jar

Click Connections -&gt; New connection.

Add values. The following are examples:

```sql
Connection Name: MariaDB via MySQL Conn
Username: root
Password: ********
Save Password: [x]

Choose MySQL tab

Hostname: localhost
Port: 3306

Click "Test Connection". It should says Status: Success.
Click Save.
Click Connect.
```

You are connected. You may run commands in the Worksheet.
For example:

use mdb;

show tables;

You should see the tables.