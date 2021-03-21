# Spider Use Cases

# Introduction

This article will cover simple working examples for some standard use cases for Spider.  The example will be illustrated using a sales opportunities table to be consistent throughout. In some cases the actual examples will be contrived but are used to illustrate the varying syntax options.

# Basic setup

Have 3 or more servers available and Install MariaDB on each of these servers:

- <strong>spider</strong> server which will act as the front end server hosting the spider storage engine.
- <strong>backend1</strong> which will act as a backed server storing data
- <strong>backend2</strong> which will act as a second backend server storing data

Follow the instructions [here](/kb/en/spider-storage-engine-overview/#installing) to enable the Spider storage engine on the spider server:

```sql
mysql  -u root -p < /usr/share/mysql/install_spider.sql
```

## Enable use of non root connections

When using the [General Query Log](/mariadb-administration/server-monitoring-logs/general-query-log/), non-root users may encounter issues when querying Spider tables.  Explicitly setting the  <a undefined>spider_internal_sql_log_off</a> system variable causes the Spider node to execute `SET sql_log_off` statements on the data nodes to enable or disable the General Query Log.  When this is done, queries issued by users without the `SUPER` privilege raise an error.

To avoid this, don't explicitly set the <a undefined>spider_internal_sql_log_off</a> system variable.

## Create accounts for spider to connect with on backend servers

Spider needs a remote connection to the backend server to actually perform the remote query. So this should be setup on each backend server. In this case 172.21.21.2 is the ip address of the spider node limiting access to just that server.

```sql
backend1> mysql
grant all on test.* to spider@'172.21.21.2' identified by 'spider';
flush privileges;
backend2> mysql
grant all on test.* to spider@'172.21.21.2' identified by 'spider';
flush privileges;
```

Now verify that these connections can be used from the spider node (here 172.21.21.3 = backend1 and 172.21.21.4 = backend2):

```sql
spider> mysql -u spider -p -h 172.21.21.3 test
spider> mysql -u spider -p -h 172.21.21.4 test
```

## Create table on backend servers

The table definition should be created in the test database on both backend1 and backend2 servers:

```sql
create table opportunities (
id int,
accountName varchar(20),
name varchar(128),
owner varchar(7),
amount decimal(10,2),
closeDate date,
stageName varchar(11),
primary key (id),
key (accountName)
) engine=InnoDB;
```

## Create server entries on spider server

While the connection information can also be specified inline in the comment, it is cleaner to define a server object representing each remote backend server connection:

```sql
create server backend1 foreign data wrapper mysql options 
(host '172.21.21.3', database 'test', user 'spider', password 'spider', port 3306);
create server backend2 foreign data wrapper mysql options 
(host '172.21.21.4', database 'test', user 'spider', password 'spider', port 3306);
```

### Unable to Connect Errors

Bear in mind, if you ever need to remove, recreate or otherwise modify the server definition for any reason, you need to also execute a `FLUSH TABLES` statement.  Otherwise, Spider continues to use the old server definition, which can result in queries raising the error

```sql
Error 1429: Unable to connect to foreign data source
```

If you encounter this error when querying Spider tables, issue a `FLUSH TABLES` statement to update the server definitions.

```sql
FLUSH TABLES;
```

# Use case 1: remote table

In this case, a spider table is created to allow remote access to the opportunities table hosted on backend1. This then allows for queries and remote dml into the backend1 server from the spider server:

```sql
create table opportunities (
id int,
accountName varchar(20),
name varchar(128),
owner varchar(7),
amount decimal(10,2),
closeDate date,
stageName varchar(11),
primary key (id),
key (accountName)
) engine=spider comment='wrapper "mysql", srv "backend1" , table "opportunities"';
```

# Use case 2: sharding by hash

In this case a spider table is created to distribute data across backend1 and backend2 by hashing the id column. Since the id column is an incrementing numeric value the hashing will ensure even distribution across the 2 nodes.

```sql
create table opportunities (
id int,
accountName varchar(20),
name varchar(128),
owner varchar(7),
amount decimal(10,2),
closeDate date,
stageName varchar(11),
primary key (id),
key (accountName)
) engine=spider COMMENT='wrapper "mysql", table "opportunities"'
 PARTITION BY HASH (id)
(
 PARTITION pt1 COMMENT = 'srv "backend1"',
 PARTITION pt2 COMMENT = 'srv "backend2"'
) ;
```

# Use case 3: sharding by range

In this case a spider table is created to distribute data across backend1 and backend2 based on the first letter of the accountName field. All accountNames that start with the letter L and prior will be stored in backend1 and all other values stored in backend2. Note that the accountName column must be added to the primary key which is a requirement of MariaDB partitioning:

```sql
create table opportunities (
id int,
accountName varchar(20),
name varchar(128),
owner varchar(7),
amount decimal(10,2),
closeDate date,
stageName varchar(11),
primary key (id, accountName),
key(accountName)
) engine=spider COMMENT='wrapper "mysql", table "opportunities"'
 PARTITION BY range columns (accountName)
(
 PARTITION pt1 values less than ('M') COMMENT = 'srv "backend1"',
 PARTITION pt2 values less than (maxvalue) COMMENT = 'srv "backend2"'
) ;
```

# Use case 4: sharding by list

In this case a spider table is created to distribute data across backend1 and backend2 based on specific values in the owner field. Bill, Bob, and Chris will be stored in backend1 and Maria and Olivier stored in backend2.  Note that the owner column must be added to the primary key which is a requirement of MariaDB partitioning:

```sql
create table opportunities (
id int,
accountName varchar(20),
name varchar(128),
owner varchar(7),
amount decimal(10,2),
closeDate date,
stageName varchar(11),
primary key (id, owner),
key(accountName)
) engine=spider COMMENT='wrapper "mysql", table "opportunities"'
 PARTITION BY list columns (owner)
(
 PARTITION pt1 values in ('Bill', 'Bob', 'Chris') COMMENT = 'srv "backend1"',
 PARTITION pt2 values in ('Maria', 'Olivier') COMMENT = 'srv "backend2"'
) ;
```

With [MariaDB 10.2](/kb/en/what-is-mariadb-102/) the following partition clause can be used to specify a default partition for all other values, however this must be a distinct partition / shard:

```sql
PARTITION partition_name DEFAULT
```