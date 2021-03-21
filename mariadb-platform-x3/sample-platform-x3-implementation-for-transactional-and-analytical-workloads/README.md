# Sample Platform X3 implementation for Transactional and Analytical Workloads

<strong>MariaDB Platform X3</strong> responds to business design challenges by integrating:

- MariaDB Server, the leading enterprise open source database;
- MariaDB ColumnStore, a columnar database for on-demand analytical processing; and
- MariaDB MaxScale, the world's most advanced database proxy.

MariaDB Server powers website and application workloads as a modern RDBMS. MariaDB Platform X3 extends MariaDB Server's capabilities to include:

- Transactional workloads (OLTP);
- Analytical workloads (OLAP); and
- the combination of the two as Hybrid Transactional and Analytical Processing (HTAP) queries.

This guide has been written for the DBA, developer and operator to help you stand up Platform X3 for HTAP queries, unleashing the ability to perform analysis across events as they are happening.  It is also a deployment that can scale from the small cluster of the examples below to accommodate more transactions, larger analytical processing and high availability.

## Platform X3 Query Routing and Data Streaming

When MariaDB Platform X3 is deployed for HTAP, web and mobile services send queries to MariaDB MaxScale.  In turn, MaxScale distributes these queries according to their purpose,  transactional queries are sent to MariaDB Servers for OLTP workloads, and analytical queries are sent to MariaDB ColumnStore for OLAP operations.

On the back-end, changes made to the MariaDB Servers are sent through MaxScale streaming data adapters to ColumnStore, ensuring that ColumnStore remains up-to-date.

<img src="/kb/en/sample-platform-x3-implementation-for-transactional-and-analytical-workloads/+image/platform-x3-routing-streaming-small" alt="platform-x3-routing-streaming-small" title="platform-x3-routing-streaming-small">

## Platform X3 Scaleout

MariaDB Platform X3 can operate from individual servers, but as your application grows more complicated and your database workload increases, each component can scale out to suit your particular infrastructure needs.

<img src="/kb/en/sample-platform-x3-implementation-for-transactional-and-analytical-workloads/+image/platform-x3-scaleout" alt="platform-x3-scaleout" title="platform-x3-scaleout">

For OLTP operations, our sample Platform X3 deployment we start with four MariaDB Servers, configured to run as one master and three slaves synchronized with each other in a MariaDB Replication cluster.  In scaling OLTP, you can increases the number of MariaDB Servers, allowing for high availability, replication backups and failover.

For OLAP operations, our sample deployment uses five MariaDB ColumnStore nodes, two of which are configured as User Modules (UM's) and three as Performance Modules (PM's).  In scaling OLAP, you can increase the number of UM's to handle more incoming queries or increase the number of PM's to better handle the processing of those queries.

To manage network communication between the application and our deployment and between the database servers we use two MaxScale servers, one to handle the back-end data streaming and the other to handle selective query proxying from your application.  In scaling for the network load, you can add MaxScale servers to the first to handle a larger database write load or to the second to manage a greater number of queries from your application.

## Sample Use Cases

### Retail Store Example

A retail store wants to amplify sales by providing a personalized shopping experience. When a customer checks out at a cash register or online, the customer is presented promotions tailored to the customer's interests. These offers may also be integrated across channels, available when the known customer visits the website, shops by mobile phone app, or be included in the next personalized email sent to the customer.

At a technical level, when an OLTP query is performed to process the customer's purchase, the customer's past and current purchase history is analyzed with an OLAP query to provide promotions tailored to the customer's buying history.

### Retail Bank Example

A retail bank maintains a customer database that includes their account information and a transaction activities ledger.  When a customer makes a deposit or a withdrawal, the application updates the transaction activities ledger of their account.  The customer can access their account information and activity through the bank's online portal.

Additionally, the application generates reports analyzing transaction activities.  These reports are adapted for categories of customers (business, student, regular checking, savings) or for types of transactions (cash deposits, checks, ATM deposits, in-branch deposits, transfers, withdrawals). These reports can be run by the customers on their individual accounts or by the bank's back office on all customer activities.

In this scenario, queries listing account information and general transaction activities are OLTP operations.  Reports analyzing transaction activities run by the customer for individual accounts or by the bank on all customers are OLAP operations.

### Internet of Things Example

A chain of convenience stores maintains an IoT (Internet of Things) network in which each store records data on its milk inventory levels and sensor data such as refrigerator temperature. The central office continuously monitors inventory levels to trigger replenishment on an as-needed basis. The maintenance teams and store also receive real-time alerts if issues arise with the cooling system, speeding repair and reducing product losses. A holistic, whole-picture view of supply levels and status allows the chain to keep costs low and the customer experience consistent.

At a technical level, purchasing of a milk carton or container triggers an OLTP query, and inventory reporting is an OLAP query. OLTP data is used for logging, and analysis of OLAP data drives understanding of product losses, replenishment patterns, and equipment failures.

## Sample Deployment

The following sections detail how to implement a sample deployment of Platform X3 for HTAP.  The first steps cover server installation and deployment; the next cover configuration for [Replication](#configure-for-replication), [Data Streaming](#configure-for-data-streaming), and [Application Traffic](#configure-for-application-traffic), and lastly [Testing](#test-htap-application-traffic) with OLTP and OLAP queries and with DML statements.

### Deploy MariaDB Servers

Our sample deployment calls for four servers running MariaDB Server to handle OLTP workloads, which we've named `Server-1` to `Server-4`.  These are shown at the left of our sample deployment diagram, in orange. We use InnoDB as the storage engine on these servers.

These four servers run MariaDB Server 10.3, installed per the instructions at: [Getting and Upgrading MariaDB](/mariadb-administration/getting-installing-and-upgrading-mariadb).

### Deploy MariaDB ColumnStore Servers

Our sample deployment calls for five servers to run MariaDB ColumnStore to handle OLAP workloads.  Two of these servers operate as User Module servers, named `UM-1` and `UM-2`, and receive application traffic from MaxScale.  The other three operate as Performance Module servers, named `PM-1` through `PM-3`, and perform distributed query processing.

These servers run MariaDB ColumnStore 1.2.2 and are installed using the non-root and non-distributed installation method, as per the instructions at: [Preparing and Installing MariaDB ColumnStore](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-getting-started/preparing-and-installing-mariadb-columnstore-12x).

### Deploy MariaDB MaxScale Servers

Our sample deployment calls for two servers to run MariaDB MaxScale.  The first server, named `MaxScale-1`, handles data streaming from the MariaDB Servers to the MariaDB ColumnStore servers.  The second, named `MaxScale-2`, selectively proxies application traffic to the respective servers for OLTP and OLAP workloads.

These servers run MariaDb MaxScale 2.3.1, installed per the instructions at: [Installing MariaDB MaxScale](mariadb-enterprise/mariadb-maxscale-23-mariadb-maxscale-installation-guide).

### Configure for Replication

Once we have the server software installed on the respective hosts, we can begin configuring them for use. To start, our sample deployment calls for the four MariaDB Servers to synchronize data using MariaDB Replication.  This allows for high availability on OLTP operations, replication backup and failover.

In MariaDB Replication, one server operates as the master receiving all writes from the application and replicating changes to the cluster.  The other servers operate as slaves, receiving reads from the application and only accepting writes from the master server.

For our sample deployment, `Server-1` operates as the replication master while `Server-2` through `Server-4` operates as the replication slaves.

#### Configure `Server-1` (master)

Add the following lines to the `[mysqld]` section of `/etc/my.cnf.d/server.cnf`:

```sql
[mysqld]
server_id        = 1
log_bin          = mariadb-bin
binlog_format    = ROW
gtid_strict_mode = 1
log_error
log-slave-updates
```

In streaming data from MariaDB Server to ColumnStore for analysis, MaxScale requires that the Servers format the binary log events by each row modified by a statement, rather than by operation.  So, when deploying a cluster for HTAP, ensure that the <a undefined>binlog_format</a> system variable on the MariaDB Servers is always set to the `ROW` value.

For more information on these server system variables and others, see [Server System Variables](/replication/optimization-and-tuning/system-variables/server-system-variables).

#### Configure `Server-2` (slave)

Add the following lines to the `[mysqld]` section of `/etc/my.cnf.d/server.cnf`:

```sql
[mysqld]
server_id        = 2
log-bin          = mariadb-bin
binlog-format    = ROW
gtid_strict_mode = 1
log_error
log-slave-updates
```

#### Configure `Server-3` (slave)

Add the following lines to the `[mysqld]` section of `/etc/my.cnf.d/server.cnf`:

```sql
[mysqld]
server_id        = 3
log-bin          = mariadb-bin
binlog-format    = ROW
gtid_strict_mode = 1
log_error
log-slave-updates
```

#### Configure `Server-4` (slave)

Add the following lines to the `[mysqld]` section of `/etc/my.cnf.d/server.cnf`:

```sql
[mysqld]
server_id        = 4
log-bin          = mariadb-bin
binlog-format    = ROW
gtid_strict_mode = 1
log_error
log-slave-updates
```

#### Restart `Server-1` to `Server-4`

Restart all four MariaDB Servers.  Log into each server (`Server-1`, `Server-2`, `Server-3` and `Server-4`), and issue the restart command to `systemctl` on each server:

```sql
# systemctl restart mariadb.service
```

#### Create a replication user on `Server-1`

When MariaDB Servers run as replication slaves, they replicate data through client connections with the master server.  In order for these servers to establish client connections, create a replication user on the master server, `Server-1`, and grant the user the relevant privileges to retrieve the data.

Connect to the master MariaDB Server through the client:

```sql
$ mysql -u root -p -h <Server-1-ip> -P 3306
```

Once connected, reset the master:

```sql
RESET MASTER;
```

Then, create a replication user for MaxScale and the slave MariaDB Servers and grant the relevant privileges to the user:

```sql
CREATE USER 'repl'@'%' IDENTIFIED BY 'pass';
GRANT REPLICATION SLAVE ON *.* TO 'repl';
GRANT SELECT ON mysql.user TO 'repl';
GRANT SELECT ON mysql.db TO 'repl';
GRANT SELECT ON mysql.tables_priv TO 'repl';
GRANT SELECT ON mysql.roles_mapping TO 'repl';
GRANT SHOW DATABASES ON *.* TO 'repl';
GRANT REPLICATION CLIENT ON *.* TO 'repl';
```

For more information on MariaDB Replication, see [Performance Tuning with MariaDB Replication](/kb/en/high-availability-performance-tuning-mariadb-replication/).

#### Configure and start replication on `Server-2` to `Server-4`

With each slave MariaDB Server in your deployment, configure it to replicate data from the master server and start the replication process.  Perform the following operations on each slave server, (that is, `Server-2` through `Server-4`).

First, connect to the slave server:

```sql
$ mysql -u root -h <slave-server-ip> -P 3306 -p
```

If replication is currently running, reset the master so you can update its configuration:

```sql
RESET MASTER;
```

Issue a [CHANGE MASTER TO](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to) statement to configure the slave to replicate from `Server-1`:

```sql
CHANGE MASTER TO
       MASTER_HOST='Server-1-ip',
       MASTER_USER='repl',
       MASTER_PASSWORD='pass';
```

Then, start the replication slave:

```sql
START SLAVE;
```

You can verify that replication is working using a <a undefined>SHOW SLAVE STATUS</a> statement.

```sql
SHOW SLAVE STATUS;
```

### Configure for data streaming

In HTAP deployments the only queries issued to MariaDB ColumnStore are those specific to OLAP workloads, which does not include writes.  In order to update ColumnStore with new data written to the MariaDB Servers, configure MaxScale on the back-end to stream writes to ColumnStore.

#### Configure MaxScale

Our sample deployment calls for a MaxScale server to handle data streaming between the MariaDB Servers and the MariaDB ColumnStore cluster.  On the `MaxScale-1` server, edit the `/etc/maxscale.cnf` configuration file, adding the following lines to set it up for data streaming to ColumnStore using the Avro Listener.

First, configure the replication router.  Set it to use the particular `server_id` and the binary log:

```sql
[replication-router]
type                    = service
router                  = binlogrouter
user                    = repl
password                = pass
server_id               = 5
master_id               = 1
Binlogdir               = /var/lib/maxscale
Mariadb10-compatibility = 1
filestem                = mariadb-bin
```

Then, configure the replication listener:

```sql
[replication-listener]
type     = listener
service  = replication-router
protocol = MySQLClient
port     = 6603
```

MaxScale now listens for configuration connections on port 6603.  This allows a MariaDB client to set up replication using commands similar to those that manage a replication slave server.  It only uses the user and password to authentication the configuration connection, (the credentials for connecting to the Server are specified in the [configuration](#configure-maxscale-as-a-slave) below).

Next, configure the router for the Avro service.

```sql
[avro-router]
type    = service
router  = avrorouter
source  = replication-router
avrodir = /var/lib/maxscale
```

This generates JSON files from the binary logs it receives from the master MariaDB Server and stores them in the `avodir` directory (that is, `/var/lib/maxscale/`) using the replication router.

Then, configure the listener for the Avro service to use a specific port:

```sql
[avro-listener]
type     = listener
service  = avro-router
protocol = cdc
port     = 4001
```

Once this is done, save the file and restart MaxScale to apply the new configuration:

```sql
# sudo systemctl restart maxscale
```

Lastly, using the `maxctrl` utility, create a user for the Avro Router to capture data changes.  This user handles streaming data MaxScale retrieves from the MariaDB Servers to ColumnStore.

```sql
# maxctrl call command cdc add_user avro-router cdcuser cdc
```

For more information on the Avro Router, see: [Avro Router](/kb/en/mariadb-maxscale-21-avrorouter/).

#### Configure MaxScale as a slave

When the MaxScale server streams data to MariaDB ColumnStore it retrieves it from the master server using the same process that the slaves use in MariaDB Replication.  In effect it operates as a replication slave, only instead of writing data locally, it streams the writes to the ColumnStore User Modules.

Connect to MaxScale using the MariaDB Client.  Unlike when connecting to MariaDB Servers previously, use port 6603, (which you configured above in the `/etc/maxscale.cnf` file as the replication listener port).

```sql
$ mysql -h <MaxScale-1-ip> -P 6603 -u repl -p
```

Issue a [CHANGE MASTER TO](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to) statement to use the master MariaDB Server host (that is, the IP address to Server-1) and the port for client connections, (which defaults to 3306).  Set the user and password as defined for the replication router in `/etc/maxscale.cnf` above.

```sql
CHANGE MASTER TO MASTER_HOST='<Server-1-ip>',
       MASTER_PORT=3306, 
       MASTER_USER='repl', 
       MASTER_PASSWORD='pass', 
       MASTER_LOG_FILE='mariadb-bin.000001';
```

Then, start the slave:

```sql
START SLAVE;
```

Once you've started the replication slave process on MaxScale, you can check it using the <a undefined>SHOW SLAVE STATUS</a> statement, just as you would when checking the status of a slave MariaDB Server.

```sql
SHOW SLAVE STATUS;
```

If there are no errors, `MaxScale-1` is now running as a replication slave to `Server-1`.

#### Configure the CDC user

On the master MariaDB Server, (that is, `Server-1`), create a user for the CDC service.  The CDC Data Adapter authenticates with this user when retrieving data from the MariaDB Servers.

Connect using the MariaDB Client to `Server-1`:

```sql
$ mysql -u root -p -h <Server-1-ip>
```

Then, issue the following statements to create the CDC user and grant it the necessary privileges:

```sql
CREATE USER 'cdcuser'@'%' IDENTIFIED BY 'cdc';
GRANT REPLICATION SLAVE ON *.* TO 'cdcuser';
GRANT SELECT ON mysql.user TO 'cdcuser';
GRANT SELECT ON mysql.db TO 'cdcuser';
GRANT SELECT ON mysql.tables_priv TO 'cdcuser';
GRANT SELECT ON mysql.roles_mapping TO 'cdcuser';
GRANT SHOW DATABASES ON *.* TO 'cdcuser';
GRANT REPLICATION CLIENT ON *.* TO 'cdcuser';
```

#### Install the CDC Streaming Data Adapter

The MaxScale CDC Streaming Data Adapter allows you to stream binary log events from MariaDB Servers to MariaDB ColumnStore clusters. In order to use it, install the ColumnStore Bulk Write SDK and the MaxScale CDC Adapter packages on a dedicated host or on any MaxScale server that you want to use for data streaming, (`MaxScale-1` in our sample deployment).

Downloads are available at: [Downloads](https://mariadb.com/downloads/#mariadbax-axdataadapters).

Note these packages conflict with ColumnStore installations.  Don't install them on any of your ColumnStore servers.

To install the ColumnStore Bulk Write SDK, download the RPM package from MariaDB, then install the EPEL releases and package dependencies using YUM:

```sql
$ wget https://downloads.mariadb.com/Data-Adapters/mariadb-columnstore-api/1.2.2/centos/x86_64/7/Mariadb-columnstore-api-1.2.2-1-x86_64-centos7-cpp.rpm
$ sudo yum install epel-release
$ sudo yum install -y libuv libxml2 snappy python34
$ sudo yum install -y  mariadb-columnstore-api-1.2.2-1-x86_64-centos7-cpp.rpm
```

To install the CDC Data Adapter, download the RPM package from MariaDB, then install it using YUM:

```sql
$ wget https://downloads.mariadb.com/Data-Adapters/mariadb-streaming-data-adapters/cdc-data-adapter/1.2.2/centos-7/mariadb-columnstore-maxscale-cdc-adapters-1.2.2-1-x86_64-centos7.rpm
$ sudo yum install -y mariadb-columnstore-maxscale-cdc-adapters-1.2.2-1-x86_64-centos7.rpm
```

For more information on installing the Data Adapter, see [Data Adapters Installation](/kb/en/columnstore-streaming-data-adapters/#installation).

#### Configure the CDC Data Adapter

With the CDC Data Adapter installed you can configure it to stream data to MariaDB ColumnStore.  This is done by copying the `Columnstore.xml` configuration file from one of the ColumnStore nodes to the `MaxScale-1` server, where the CDC Data Adapter can use it.

```sql
$ scp root@columnstore-host:/home/mysql/columnstore/etc/Columnstore.xml \
      ~/Columnstore.xml
$ sudo mv Columnstore.xml /etc
```

Note, installing MaxScale and the CDC Data Adapter as root creates the `/var/lib/mxs_adapter/` directory. If you intend to run `mxs_adapter` as a non-root user, ensure that the user can read and write from this directory.  If not, change the ownership to that of your user.  For instance,

```sql
$ sudo chown ec2-user /var/lib/mxs_adapter
```

Lastly, check the firewall and SELinux.  ColumnStore uses ports 8600 to 8630, as well as ports 8700 and 8800.  The CDC Data Adapter uses the same ports to stream data from `MaxScale-1` to ColumnStore.  Check each server to ensure that there is no firewall blocking these ports.  Additionally, ensure that SELinux has a policy allowing these connections or that it is running in permissive mode.

#### Verify data streaming

Once you've installed and configured the MaxScale and the CDC Data Adapter, you can run checks to verify that it is properly configured and able to communicate and stream data from the MariaDB Servers to the MariaDB ColumnStore cluster.  Using the `mxs_adapter` utility, you can connect to MaxScale and test data streaming.

In order to test this feature, you first need to create tables to store the data the CDC Data Adapter transfers.  First, connect to the master MariaDB Server, `Server-1`, and create an InnoDB table to use in storing test data:

```sql
CREATE TABLE test.t6(a INT, b INT) ENGINE=InnoDB;
```

Then, connect to one of the User Modules and create a ColumnStore table with the same name and schema:

```sql
CREATE TABLE test.t6(a INT, b INT) ENGINE=ColumnStore;
```

In order to stream data from the MariaDB Servers to ColumnStore,call the `mxs_adapter` utility.  From the `MaxScale-1` server, run the following command:

```sql
$ mxs_adapter -c /etc/ColumnStore.xml -u cdcuser -p cdc \
  	-h localhost -P 4001 -r 2 -d -n -z test t6
```

Use the username and password for the CDC user created in the previous section.  In the MaxScale configuration, we set port 4001 for the listener service.

The last two arguments tell it to stream the `t6` table on the `test` database.  If you want to stream multiple tables, replace these arguments with a `-f` option that gives the path to a table list file.  Format the file for one table per line, separating the database name and table name by a tab.  For instance,

```sql
$ cat tbl.lst
test t6
test t7
test t8
```

When you start streaming data, the `mxs_adapter` utility begins printing logging messages to stdout.  As you add data to the MariaDB Servers, you can check this output to see binary events streaming over to ColumnStore.

To test this, begin inserting data onto the `test.t6` table on the `Server-1` server.  This is the master server in the MariaDB Replication and the only one that accepts write operations.

```sql
INSERT INTO test.t6 VALUES (1,1);

INSERT INTO test.t6 VALUES (1,2);

INSERT INTO test.t6 VALUES (1,3);

INSERT INTO test.t6 VALUES (1,4);

INSERT INTO test.t6 VALUES (1,5);
```

Then, issue a [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select) statement to see what data is available on the MariaDB Servers for the OLTP operations:

```sql
SELECT * FROM test.t6;
+----+----+
| a  | b  |
+----+----+
|  1 |  1 |
|  1 |  2 |
|  1 |  3 |
|  1 |  4 |
|  1 |  5 |
+----+----+
5 rows in set (0.010 sec)
```

By checking the logging messages coming from the `mxs_adapter` utility, you can watch these [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert) statements as they stream from the MariaDB Servers through MaxScale to ColumnStore:

```sql
2018-11-23 18:39:09 [main] Started thread 0x176b470
2018-11-23 18:39:09 [main] Started 1 threads
2018-11-23 18:39:09 [test.t6] Requesting data for table: test.t6
2018-11-23 18:39:09 [test.t6] INSERT INTO `test`.`t6` (`a`,`b`) VALUES (1,1)
2018-11-23 18:39:09 [test.t6] DML average: 12ms
2018-11-23 18:39:19 [test.t6] Read timeout
2018-11-23 18:39:22 [test.t6] INSERT INTO `test`.`t6` (`a`,`b`) VALUES (1,2)
2018-11-23 18:39:22 [test.t6] DML average: 4ms
2018-11-23 18:39:24 [test.t6] INSERT INTO `test`.`t6` (`a`,`b`) VALUES (1,3)
2018-11-23 18:39:24 [test.t6] DML average: 4ms
2018-11-23 18:39:26 [test.t6] INSERT INTO `test`.`t6` (`a`,`b`) VALUES (1,4)
2018-11-23 18:39:26 [test.t6] DML average: 4ms
2018-11-23 18:39:28 [test.t6] INSERT INTO `test`.`t6` (`a`,`b`) VALUES (1,5)
2018-11-23 18:39:28 [test.t6] DML average: 9ms
```

Then, you can issue a similar [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select) statement to the `test.t6` table on either of the User Modules for MariaDB ColumnStore to see that the data is now available for OLAP operations:

```sql
SELECT * FROM test.t6;
+----+----+
|  a | b  |
+----+----+
|  1 |  1 |
|  1 |  2 |
|  1 |  3 |
|  1 |  4 |
|  1 |  5 |
+----+----+
5 rows in set (0.010 sec)
```

Next, test out other write operations using either an [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update) or [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete) statement on `Server-1`:

```sql
UPDATE test.t6 SET a = 2 WHERE b > 3;

SELECT * FROM test.t6;
+----+----+
| a  | b  |
+----+----+
|  1 |  1 |
|  1 |  2 |
|  1 |  3 |
|  2 |  4 |
|  2 |  5 |
+----+----+
5 rows in set (0.000 sec)
```

Using the logging messages from the CDC Data Adapter, you can watch the binary events for this operation stream through MaxScale:

```sql
2018-11-23 18:44:18 [test.t6] Read timeout
2018-11-23 18:44:22 [test.t6] UPDATE `test`.`t6` SET `a` = 2, `b` = 4 WHERE `a` = 1 AND `b` = 4
2018-11-23 18:44:22 [test.t6] DML average: 12ms
2018-11-23 18:44:22 [test.t6] UPDATE `test`.`t6` SET `a` = 2, `b` = 5 WHERE `a` = 1 AND `b` = 5
2018-11-23 18:44:22 [test.t6] DML average: 13ms
2018-11-23 18:44:27 [test.t6] Read timeout
```

As you can see from the logging messages, MaxScale detected the [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update) statement and streamed it through the CDC Data Adapter to ColumnStore.  The CDC Data Adapter then begins logging `Read timeout` messages to indicate that it is done streaming and is waiting on additional binary events from the MariaDB Servers.

You can confirm that the data was successfully transferred by issuing a [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select) statement to one of the MariaDB ColumnStore User Modules:

```sql
SELECT * FROM test.t6;
+----+----+
| a  | b  |
+----+----+
|  1 |  1 |
|  1 |  2 |
|  1 |  3 |
|  2 |  4 |
|  2 |  5 |
+----+----+
5 rows in set (0.011 sec)
```

Notice that rows 4 and 5 now contain new values.

### Configure for Application Traffic

When your application issues queries to Platform X3 for HTAP operations, it doesn't connect to either the MariaDB Servers or to the MariaDB ColumnStore User Modules directly.  Instead, it connects to a MaxScale server configured to selectively routes queries, ensuring that OLTP operations execute on MariaDB Servers and OLAP operations execute on ColumnStore.

In order to better illustrate how MaxScale distributes queries between the servers, we are going to install a sample banking database and show how to process payments and analyze loan data.

Our sample database contains the following tables:

- `account` - each record describes the static characteristics of an account
- `client` - each record describes the characteristics of a client
- `client_accts` - each record relates together a client with an account
- `loan` - each record describes a loan granted for a given account

<img src="/kb/en/sample-platform-x3-implementation-for-transactional-and-analytical-workloads/+image/platform-x3-bank-db" alt="platform-x3-bank-db" title="platform-x3-bank-db">

With the configuration so far, our sample deployment will streams these new tables from the MariaDB Serves to ColumnStore through the CDC Data Adapter running on `MaxScale-1`.  We'll then configure the second MaxScale server to selectively route your application traffic for HTAP operations.

#### Prepare MariaDB Server and MariaDB ColumnStore to receive traffic from MaxScale

Create a read user for MaxScale on the master MariaDB Server `Server-1` and the ColumnStore User Modules.  Grant it the necessary privileges to operate.

```sql
CREATE USER 'maxscale' IDENTIFIED BY 'pass';
GRANT SELECT ON mysql.user TO 'maxscale';
GRANT SELECT ON mysql.db TO 'maxscale';
GRANT SELECT ON mysql.tables_priv TO 'maxscale';
GRANT SHOW DATABASES ON *.* TO 'maxscale';
```

Then, create a write user for MaxScale on the same servers.

```sql
CREATE USER 'maxuser'@'%' IDENTIFIED BY 'maxpwd';
GRANT ALL ON *.* TO 'maxuser'@'%';
```

#### Create the schema

Download and unzip the sample dataset onto both Server-1 and a ColumnStore User Module from [test-db](https://drive.google.com/a/mariadb.com/file/d/1At6q7qSKJkCe-NcwDCQrkjpDisXVOIDn/view?usp=sharing). This file is `test-db.zip`, which unzips into the `test-db/` directory .  It contains following SQL and CSV files:

```sql
$ unzip test-db.zip
$ ls test-db/
create-db-cs.sql
create-db-innodb.sql 
account.csv  
client_accts.csv  
client.csv  
loan.csv
```

On the master MariaDB Server, `Server-1`, change into the unzipped directory and launch the client as the write user `maxuser` you created above for MaxScale:

```sql
$ cd test-db
$ mysql -u 'maxuser' -p
```

Use the <a undefined>SOURCE</a> command to load the `create-db-innodb.sql` file, initializing the base database:

```sql
SOURCE create-db-innodb.sql;
```

Then, modify the schema to add a balance column to the `bank.loan` table.  This column will be used in the ColumnStore examples below.

```sql
SET sql_log_bin = 0;
ALTER TABLE bank.loan ADD COLUMN balance decimal(10,2);
SET sql_log_bin = 1;
```

On a ColumnStore User Module, connect from the same directory and with the same user:

```sql
$ cd test-db
$ mysql -u 'maxuser' -p 
```

Then, use the <a undefined>SOURCE</a> command to load the `create-db-cs.sql` file:

```sql
SOURCE create-db-cs.sql;
```

At this point, we have created the bank database and tables, and have loaded the data into the MariaDB Servers, (though we only wrote to `Server-1`, as the master server it has replicated the data out to the slaves). We have also created the bank database and tables in MariaDB ColumnStore. The tables on MariaDB ColumnStore are empty at this point.

#### Streaming Data from MariaDB Servers to MariaDB ColumnStore

Next, we will start streaming data via the `mxs_adapter` utility, so that the data we've loaded on the MariaDB Servers can stream to MariaDB ColumnStore. On `MaxScale-1`, create a TSV (tab-separated) file named bank.lst with the bank database tables we want to stream:

```sql
$ cat bank.lst
bank	account
bank	client
bank	client_accts
bank	loan
```

Now, start the `mxs_adapter` utility designating this file with the `-f` option for this file:

```sql
$ mxs_adapter -c /etc/Columnstore.xml -u cdcuser -p cdc \
      -h <maxscale-1-host> -P 4001 -r 50 -d -n -f bank.lst
```

When you run the `mxs_adapter` utility, it streams logging messages about the operations it's performing to stdout.  You can monitor this information to see the binary events its streaming from the MariaDB Servers to MariaDB ColumnStore.

```sql
2018-11-28 03:56:49 [bank.client_accts] client_id: 13971
2018-11-28 03:56:49 [bank.client_accts] account_id: 11362
2018-11-28 03:56:49 [bank.client_accts] ca_type: OWNER
2018-11-28 03:56:49 [bank.client_accts] ca_id: 13690
2018-11-28 03:56:49 [bank.client_accts] client_id: 13998
2018-11-28 03:56:49 [bank.client_accts] account_id: 11382
2018-11-28 03:56:49 [bank.client_accts] ca_type: OWNER
2018-11-28 03:56:49 [bank.client_accts] ca_id: 0
2018-11-28 03:56:49 [bank.client_accts] client_id:
2018-11-28 03:56:49 [bank.client_accts] account_id:
2018-11-28 03:56:49 [bank.client_accts] ca_type:
2018-11-28 03:56:52 [bank.account] Read timeout
2018-11-28 03:56:57 [bank.loan] Read timeout
2018-11-28 03:56:58 [bank.client] Flushing batch
2018-11-28 03:56:58 [bank.client] 21 rows, 0 transactions inserted over 10.1008 seconds. GTID = 0-1-23:5371
2018-11-28 03:56:59 [bank.client_accts] Flushing batch
2018-11-28 03:56:59 [bank.client_accts] 21 rows, 0 transactions inserted over 10.1153 seconds. GTID = 0-1-27:5371
2018-11-28 03:57:02 [bank.account] Read timeout
```

When all the loaded data has been streamed from the MariaDB Servers to ColumnStore, you'll begin to see `Read timeout` messages in the output.  This means that the `mxs_adapter` utility is now waiting on additional binary events to occur on the MariaDB Servers.

At this point, if you issue a [SELECT COUNT(*)](/built-in-functions/aggregate-functions/count) statements to MariaDB ColumnStore, you should get following result-sets:

```sql
SELECT COUNT(*) FROM bank.account;
+----------+
| COUNT(*) |
+----------+
|     4502 |
+----------+
1 row in set (0.057 sec)

SELECT COUNT(*) FROM bank.client;
+----------+
| COUNT(*) |
+----------+
|     5371 |
+----------+
1 row in set (0.051 sec)

SELECT COUNT(*) FROM bank.loan;
+----------+
| COUNT(*) |
+----------+
|      684 |
+----------+
1 row in set (0.056 sec)

SELECT COUNT(*) FROM bank.client_accts;
+----------+
| COUNT(*) |
+----------+
|     5371 |
+----------+
1 row in set (0.027 sec)
```

The MariaDB Servers and ColumnStore now contain the same data.

#### Configure HTAP routing

Our sample deployment proxies application traffic through a second MariaDB MaxScale server for the selective routing of HTAP queries.  Specifically, it involves configuring the MaxScale-2 server so that:

- All [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert), [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update), [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete) statements always route to `Server-1` for (OLTP)
- All [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select) queries on the loan table always route to MariaDB ColumnStore (OLAP)
- All remaining read queries will route to any MariaDB Server.

Below is a diagram of the `MaxScale-2` server configuration:

<img src="/kb/en/sample-platform-x3-implementation-for-transactional-and-analytical-workloads/+image/platform-x3-htap-routing" alt="platform-x3-htap-routing" title="platform-x3-htap-routing">

Here is the configuration file you should have in `/etc/maxscale.cnf` on `MaxScale-2` to achieve the above.

```sql
[maxscale]
threads  = auto
sql_mode = default

## Identify the Master MariaDB Server: Server-1
[sw-db1]
type     = server
address  = <MariaDB-Server-1-IP>
port     = 3306
protocol = MariaDBBackend

## Identify the Slave MariaDB Server: Server-2
[sw-db2]
type     = server
address  = <MariaDB-Server-2-IP>
port     = 3306
protocol = MariaDBBackend

## Identify the Slave MariaDB Server: Server-3
[sw-db3]
type     = server
address  = <MariaDB-Server-3-IP>
port     = 3306
protocol = MariaDBBackend

## Identify the Columnstore Server: UM-1
[sw-mcs-um1]
type     = server
address  = <MariaDB-UM-1-IP>
port     = 3306
protocol = MariaDBBackend

## Monitor all servers
[MariaDB-Monitor]
type             = monitor
module           = mariadbmon
servers          = sw-db1,sw-db2,sw-db3,sw-mcs-um1
user             = maxuser
password         = maxpwd
monitor_interval = 10000

## Service to talk to the servers.
[MDB-Service]
type      = service
router    = readwritesplit
servers   = sw-db1,sw-db2
user      = maxuser
password  = maxpwd

## Listener that clients use to access the MariaDB Servers.
[MDB-Listener]
type     = listener
service  = MDB-Service
protocol = MariaDBClient
port     = 4009

## The MDB-Service abstracted as a server
[MDB-Service-as-server]
type     = server
address  = 127.0.0.1
port     = 4009
protocol = MariaDBBackend

## Service to talk to the ColumnStore UM
[CS-Service]
type           = service
router         = readconnroute
router_options = running
servers        = sw-mcs-um1
user           = maxuser
password       = maxpwd

## Listener that clients use to access the CS-Service.
[CS-Listener]
type     = listener
service  = CS-Service
protocol = MariaDBClient
port     = 4010

## CS-Service abstracted as a server
[CS-Service-as-server]
type     = server
address  = 127.0.0.1
port     = 4010
protocol = MariaDBBackend

## Filter the _datamart_ queries to the ColumnStore server and rest of the queries to the MariaDB Servers
[target-selector]
type     = filter
module   = namedserverfilter
match01  = (?i)SELECT.*loan
target01 = CS-Service-as-server
match02  = .*
target02 = MDB-Service-as-server

## Filter to replace loan table name without database qualifier, with bank.loan, so that the connection to ColumnStore knows which database to use
[loan-table-filter]
type      = filter
module    = regexfilter
options   = ignorecase
match     = \sloan\s
replace   = /* */  bank.loan /* */
log_trace = true
log_file  = /tmp/regexfilter.log

## Combines the two services as one
[HTAP-Service]
type                   = service
Router                 = schemarouter
ignore_databases_regex = .*
servers                = MDB-Service-as-server,CS-Service-as-server
Preferred_server       = MDB-Service-as-server
user                   = maxuser
password               = maxpwd
filters                = target-selector

## Listener clients use to access the combined service
[HTAP-Listener]
type     = listener
service  = HTAP-Service
protocol = MariaDBClient
port     = 4011
```

Then, start MaxScale:

```sql
# systemctl start maxscale
```

### Test HTAP Application Traffic

With the MariaDB Servers, ColumnStore and MaxScale servers configured and deployed, you can begin testing the sample HTAP deployment.  The application connects to the second MaxScale server, `MaxScale-2`, on port 4011, where it performs selective query routing:

- All [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert), [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update), [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete) queries always route to `Server-1` (OLTP)
- All [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select) queries on loans table always route to MariaDB ColumnStore (OLAP)
- All remaining read queries route to any MariaDB Server (OLTP)

#### Connect from application to MaxScale HTAP Service

From your application server use the MariaDB Client to connect to the MaxScale HTAP Service.

```sql
$ mysql -h <maxscale2-host> -P 4011 -u maxuser -p
```

These are the same command-line options as you would use to connect to a MariaDB Server, but instead of an individual server, you connect to MaxScale, which sends the queries to the Servers or to one of the ColumnStore UM's.

Use the password '`maxpwd`' value that you previously set for this user.

#### Transactional queries

The MariaDB MaxScale server configuration above designates queries on tables other than bank.loan as transactional and routes them to the MariaDB Servers rather than ColumnStore.  You can identify which server cluster the query executes on using the <a undefined>version_comment</a> system variable.

```sql
SELECT *, @@version_comment FROM bank.account LIMIT 5;
+------------+-------------+------------------+------------+-------------------+
| account_id | district_id | frequency        | a_d        | @@version_comment |
+------------+-------------+------------------+------------+-------------------+
|        576 |          55 | POPLATEK MESICNE | 1993-01-01 | MariaDB Server    |
|       3818 |          74 | POPLATEK MESICNE | 1993-01-01 | MariaDB Server    |
|        704 |          55 | POPLATEK MESICNE | 1993-01-01 | MariaDB Server    |
|       2378 |          16 | POPLATEK MESICNE | 1993-01-01 | MariaDB Server    |
|       2632 |          24 | POPLATEK MESICNE | 1993-01-02 | MariaDB Server    |
+------------+-------------+------------------+------------+-------------------+
5 rows in set (0.039 sec)
```

Since MaxScale routes this query as a transactional operation, the <a undefined>version_comment</a> system variable returns MariaDB Server.

#### Analytical queries

The MariaDB MaxScale server configuration above designates queries on the bank.loans table as analytical queries and routes them to the MariaDB ColumnStore User Modules rather than the MariaDB Servers.  You can identify which server cluster the query executes on using the <a undefined>version_comment</a> system variable.

```sql
SELECT loan_id, account_id, amount, duration, payments,  @@version_comment 
FROM loan LIMIT 5;
+---------+------------+-----------+----------+----------+---------------------+
| loan_id | account_id | amount    | duration | payments | @@version_comment   |
+---------+------------+-----------+----------+----------+---------------------+
|       0 |          0 |      0.00 |        0 |     0.00 | Columnstore 1.2.1-1 |
|    5314 |       1787 |  96396.00 |       12 |  8033.00 | Columnstore 1.2.1-1 |
|    5316 |       1801 | 165960.00 |       36 |  4610.00 | Columnstore 1.2.1-1 |
|    6863 |       9188 | 127080.00 |       60 |  2118.00 | Columnstore 1.2.1-1 |
|    5325 |       1843 | 105804.00 |       36 |  2939.00 | Columnstore 1.2.1-1 |
+---------+------------+-----------+----------+----------+---------------------+
```

Since MaxScale routes the query as an analytical operation, the <a undefined>version_comment</a> system variable indicates a ColumnStore server.

#### DML Statements

The MariaDB MaxScale server configuration above designates data manipulation statements such as [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert), [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update) and [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete) as transactional and routes these statements to the MariaDB Servers.  The other MaxScale server then streams the changes over to ColumnStore.

Issue an [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update) statement to set the initial balance for a loan:

```sql
UPDATE bank.loan SET balance = amount WHERE loan_id = 5314;
```

Then, query the updated row to see the changes on ColumnStore:

```sql
SELECT loan_id, account_id, amount, payments, @@version_comment 
FROM bank.loan WHERE loan_id = 5314;
+---------+------------+----------+----------+-----------+---------------------+
| loan_id | account_id | amount   | payments |  balance  | @@version_comment   |
+---------+------------+----------+----------+-----------+---------------------+
|    5314 |       1787 | 96396.00 |  8033.00 |  96369.00 | Columnstore 1.2.1-1 |
+---------+------------+----------+----------+-----------+---------------------+
1 row in set (0.113 sec)
```

The loan balance has been set to its initial value of the received funds.

As you can see, the account holder has made payments on the loan.  This amount should be removed from the balance to reflect their paying it down.  Issue another [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update) statement to modify the balance, removing payments made on the account:

```sql
UPDATE bank.loan SET balance = balance - payments WHERE loan_id = 5314;
```

Then, query the loan to view the updated rows on ColumnStore:

```sql
SELECT loan_id, account_id, amount, payments, @@version_comment 
FROM bank.loan WHERE  loan_id = 5314;
+---------+------------+----------+----------+----------+---------------------+
| loan_id | account_id | amount   | payments | balance  | @@version_comment   |
+---------+------------+----------+----------+----------+---------------------+
|    5314 |       1787 | 96396.00 |  8033.00 | 88336.00 | Columnstore 1.2.1-1 |
+---------+------------+----------+----------+----------+---------------------+
1 row in set (0.094 sec)
```

The loan balance has now been reduced to reflect account payments.  MaxScale has also streamed the data from the changes over to ColumnStore.

## For More Information

### Obtaining the Products

When you're ready to install MariaDB Platform X3, go to [Downloads](https://mariadb.com/downloads) and select Platform X3.  If you use an RPM or APT based distribution of Linux, you can configure your server repositories to install it through the package manager.

### Documentation

Documentation for MariaDB products is available in the [Library](https://mariadb.com/kb/en/library).  For the documentation on specific products, see the links below:

- <strong>[MariaDB Server Documentation](https://mariadb.com/kb/en/library/documentation/)</strong>
<ul start="1"><li><strong>[Getting Started](/mariadb-administration/getting-installing-and-upgrading-mariadb)</strong>
</li><li><strong>[SQL Statements](/sql-statements-structure)</strong>
</li><li><strong>[SQL Functions](/built-in-functions)</strong>
</li><li><strong>[MariaDB Replication](/kb/en/high-availability-performance-tuning-mariadb-replication/)</strong>
</li><li><strong>[Server System Variables](/replication/optimization-and-tuning/system-variables/server-system-variables)</strong>
</li></ul>
- <strong>[MariadB ColumnStore Documentation](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore)</strong>
<ul start="1"><li><strong>[Getting Started](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-getting-started)</strong>
</li><li><strong>[ColumnStore Streaming Data Adapters](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-data-ingestion/columnstore-streaming-data-adapters)</strong>
</li></ul>
- <strong>[MariaDB MaxScale Documentation](mariadb-enterprise/maxscale)</strong>