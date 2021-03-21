# Spider Installation

##### MariaDB starting with [10.0.4](/kb/en/mariadb-1004-release-notes/)

The Spider storage engine was first released in [MariaDB 10.0.4](/kb/en/mariadb-1004-release-notes/).

The Spider storage engine supports partitioning and XA transactions, and allows tables of different MariaDB instances to be handled as if they were on the same instance.

To use Spider, you need two or more instances of MariaDB, typically running on separate hosts.  The Spider node is the MariaDB server that receives queries from your application.  It then processes these queries, connecting to one or more data nodes.  The data nodes are the MariaDB servers that actually store the table data.

In order for this to work, you need to configure the data nodes to accept queries from the Spider node and you need to configure the Spider node to use the data nodes as remote storage.

You don't need to install any additional packages to use it, but it does require some configuration.

## Configuring Data Nodes

Spider deployments use data nodes to store the actual table data.  In order for a MariaDB server to operate as a data node for Spider, you need to create a table or tables on which to store the data and configure the server to accept client connections from the Spider node.

For instance, first create the table:

```sql
CREATE TABLE test.spider_example (
   id INT PRIMARY KEY AUTO_INCREMENT,
   name VARCHAR(50)
) ENGINE=InnoDB;
```

Next, create a user for the Spider node and set a password for that user.  For the sake of the example, assume the Spider node is at the IP address 192.168.1.1:

```sql
CREATE USER spider@192.168.1.1;

SET PASSWORD FOR spider@192.168.1.1 = PASSWORD('passwd');
```

Then grant the `spider` user privileges on the example table.

```sql
GRANT ALL ON test.spider_example TO spider@192.168.1.1;
```

The data node is now ready for use.  You can test it by attempting to connect the MariaDB client to the data from the Spider node.  For instance, assuming the data node is at the IP address 192.168.1.5, SSH into the Spider node then try to establish a client connection.

```sql
$ mysql -u spider -p -h 192.168.1.5 test -e "SHOW TABLES;"

+----------------+
| Tables_in_test |
+----------------+
| spider_example |
+----------------+
```

## Configuring Spider Nodes

With the data node or data nodes configured, you can set up the Spider node to use them.  The Spider node is the MariaDB server that receives queries for the table, (in this case `test.spider_example`).  It then uses the Spider storage engine to connect to the tables on the data nodes to retrieve data and return the result-set.

### Node Configuration

In order to use Spider, you need to run the configuration script on the Spider node.  You can check whether it has been installed already by querying the Information Schema.

```sql
SELECT ENGINE, SUPPORT FROM information_schema.ENGINES;

+--------------------+---------+
| ENGINE             | SUPPORT |
+--------------------+---------+
| CSV                | YES     |
| MyISAM             | YES     |
| BLACKHOLE          | YES     |
| FEDERATED          | YES     |
| MRG_MyISAM         | YES     |
| ARCHIVE            | YES     |
| MEMORY             | YES     |
| PERFORMANCE_SCHEMA | YES     |
| Aria               | YES     |
| InnoDB             | DEFAULT |
+--------------------+---------+
```

If Spider does not appear in this list, you need to run the configuration script. This is installed with the MariaDB server in CentOS. Debian distributions need to install the <code class="fixed" style="white-space:pre-wrap">mariadb-plugin-spider</code> package first. The package is usually found in `/usr/share/mysql`.

```sql
SOURCE /usr/share/mysql/install_spider.sql
```

Note: If you're running a source code distribution of MariaDB, (that is, where you compiled it from source), the script is located in the build directory at `storage/spider/scripts/install_spider.sql`.

To confirm that it has been installed, query the Information Schema again,

```sql
SELECT ENGINE, SUPPORT FROM information_schema.ENGINES
WHERE ENGINE = 'SPIDER';

+--------------------+---------+
| ENGINE             | SUPPORT |
+--------------------+---------+
| SPIDER             | YES     |
+--------------------+---------+
```

Running this configuration script also creates a series of new tables in the `mysql` database, including:

<table><tbody><tr><th>table name</th><th>added version</th></tr>
<tr><td>spider_xa</td><td><a href="/kb/en/mariadb-1004-release-notes/">MariaDB 10.0.4</a></td></tr>
<tr><td>spider_xa_member</td><td><a href="/kb/en/mariadb-1004-release-notes/">MariaDB 10.0.4</a></td></tr>
<tr><td>spider_xa_failed_log</td><td><a href="/kb/en/mariadb-1005-release-notes/">MariaDB 10.0.5</a></td></tr>
<tr><td>spider_tables</td><td><a href="/kb/en/mariadb-1004-release-notes/">MariaDB 10.0.4</a></td></tr>
<tr><td>spider_link_mon_servers</td><td><a href="/kb/en/mariadb-1004-release-notes/">MariaDB 10.0.4</a></td></tr>
<tr><td>spider_link_failed_log</td><td><a href="/kb/en/mariadb-1004-release-notes/">MariaDB 10.0.4</a></td></tr>
<tr><td>spider_table_position_for_recovery</td><td><a href="/kb/en/mariadb-1033-release-notes/">MariaDB 10.3.3</a></td></tr>
<tr><td>spider_table_sts</td><td><a href="/kb/en/mariadb-1033-release-notes/">MariaDB 10.3.3</a></td></tr>
<tr><td>spider_table_crd</td><td><a href="/kb/en/mariadb-1033-release-notes/">MariaDB 10.3.3</a></td></tr>
</tbody></table>

It also upgrades the format of the tables if upgrading from an earlier version.

### Configure the Server

In order to connect the Spider node to the data nodes, you need to issue a [CREATE SERVER](/sql-statements-structure/sql-statements/data-definition/create/create-server/) statement for each data node.  You can then use the server definition in creating the Spider table.

```sql
CREATE SERVER dataNode1 FOREIGN DATA WRAPPER mysql
OPTIONS (
   HOST '192.168.1.5',
   DATABASE 'test',
   USER 'spider',
   PASSWORD 'passwd',
   PORT 3306);
```

In the event that you need to modify or replace this server after setting up the Spider table, remember to issue a [FLUSH](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/) statement to update the server definition.

```sql
FLUSH TABLES;
```

### Create the Table

With the data nodes set up and the Spider node configured for use, you can create the Spider table.  The Spider table must have the same column definitions as the InnoDB tables on the data nodes.  Spider is configured through table system variables passed to the `COMMENT` option.

```sql
CREATE TABLE test.spider_example (
   id INT PRIMARY KEY AUTO_INCREMENT,
   name VARCHAR(50)
) ENGINE=Spider
COMMENT='wrapper "mysql", srv "dataNode1", table "spider_example"';
```

This configures Spider to use the server `dataNode1`, (defined above), as a remote table.  Any data you write to this table is actually stored on the MariaDB server at 192.168.1.5.