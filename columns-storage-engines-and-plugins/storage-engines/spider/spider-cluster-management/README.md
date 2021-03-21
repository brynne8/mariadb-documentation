# Spider Cluster Management

## Direct SQL

Direct SQL is a way to map reduced execution on remote backends and store the results in a local table. This can either be sequential, using the UDF function [spider_direct_sql](/columns-storage-engines-and-plugins/storage-engines/spider/spider-functions/spider_direct_sql/), or concurrently, using [spider_bg_direct_sql](/columns-storage-engines-and-plugins/storage-engines/spider/spider-functions/spider_bg_direct_sql/).

```sql
spider1 backend << EOF 
CREATE TEMPORARY TABLE res
(
  id int(10) unsigned NOT NULL,
  k int(10) unsigned NOT NULL DEFAULT '0',
  c char(120) NOT NULL DEFAULT '',
  pad char(60) NOT NULL DEFAULT ''
) ENGINE=MEMORY;
 
SELECT spider_direct_sql(
'SELECT * FROM sbtest s  WHERE s.id IN(10,12,13)',
  'res',   
  concat('host "', host, '", port "', port, '", user "', username, '", password "', password, '", database "', tgt_db_name, '"')
) a 
FROM 
  mysql.spider_tables 
WHERE 
  db_name = 'backend' and table_name like 'sbtest#P#pt%';

SELECT * FROM res; 
EOF
```

Or if you are using a [SERVER](/sql-statements-structure/sql-statements/data-definition/create/create-server/):

```sql
SELECT spider_direct_sql( 
  'SELECT * FROM sbtest s  WHERE s.id IN(10,12,13)',
  'res',
  concat('server "', server, '"')
) a
  FROM mysql.spider_tables
  WHERE db_name = 'backend' and table_name like 'sbtest#P#pt%' ;
```

The default for [spider_bg_direct_sql](/columns-storage-engines-and-plugins/storage-engines/spider/spider-functions/spider_bg_direct_sql/) is to access concurrently all backends. If you have multiple partitions store inside a single backend, you still can increase parallelism affecting different channels to each partitions.

```sql
CREATE TEMPORARY TABLE res
(
  id int(10) unsigned NOT NULL , 
  col_microsec DATETIME(6) default NOW(8),
  db varchar(20)
) ENGINE=MEMORY;

SELECT spider_bg_direct_sql( 'SELECT count(*) ,min(NOW(6)),min(DATABASE())) FROM sbtest',   'res',  concat('srv "', server,'" cch ',@rn:=@rn+1  ) ) a  FROM    mysql.spider_tables,(SELECT @rn:=1) t2  WHERE    db_name = 'bsbackend' and table_name like 'sbtest#P#pt%';
```

## Direct Handler Socket

Check that [Handler Socket](/sql-statements-structure/nosql/handlersocket/) is running on the backend nodes

```sql
:~# backend2 -e "show variables like 'handler%'"
+-------------------------------+---------------+
| Variable_name                 | Value         |
+-------------------------------+---------------+
| handlersocket_accept_balance  | 0             |
| handlersocket_address         | 192.168.0.201 |
| handlersocket_backlog         | 32768         |
| handlersocket_epoll           | 1             |
| handlersocket_plain_secret    |               |
| handlersocket_plain_secret_wr |               |
| handlersocket_port            | 20500         |
| handlersocket_port_wr         | 20501         |
| handlersocket_rcvbuf          | 0             |
| handlersocket_readsize        | 0             |
| handlersocket_sndbuf          | 0             |
| handlersocket_threads         | 4            |
| handlersocket_threads_wr      | 1             |
| handlersocket_timeout         | 300           |
| handlersocket_verbose         | 10            |
| handlersocket_wrlock_timeout  | 12            |
+-------------------------------+---------------+
```

```sql
spider1 backend << EOF 
CREATE TEMPORARY TABLE res
(
  id int(10) unsigned NOT NULL,
  k int(10) unsigned NOT NULL DEFAULT '0',
  c char(120) NOT NULL DEFAULT '',
  pad char(60) NOT NULL DEFAULT ''
) ENGINE=MEMORY;
 
SELECT spider_direct_sql('1\t=\t1\t2\t100000\t0','res', 'host "192.168.0.202", table "sbtest", database "test", port "20500", access_mode "1"');
```

## Inter Nodes Copy Table

The UDF function [spider_copy_tables](/columns-storage-engines-and-plugins/storage-engines/spider/spider-functions/spider_copy_tables/) is available for copying table data from the source link ID to the destination link ID list without stopping your service for copying

`spider_copy_tables(Spider table name, source link ID, destination link ID list[, parameters])`

- `Returns 1` if copying data succeeded.
- `Returns 0` if copying data failed.

If the Spider table is partitioned, you must set "Spider table name" with a part name such as "table_name#P#part_name".

You can check the table name and the link ID with the part name using the following SQL:

```sql
SELECT table_name FROM mysql.spider_tables;
```

## Resharding

## General Log

To capture all queries sent to remote backends on a `Spider Node` :

```sql
SET GLOBAL general_log=ON; 
SET GLOBAL spider_general_log=ON;
SET GLOBAL spider_log_result_errors=1;
SET GLOBAL spider_log_result_error_with_sql=3;
```

## Compiling in Debug Mode

Compile MariaDB in debug mode

```sql
#cmake -DBUILD_CONFIG=mysql_release -DCMAKE_BUILD_TYPE=Debug  -DWITH_FAST_MUTEXES=ON  -DWITH_VALGRIND=ON
```

Run MariaDB the following way to have a detailed command trace file

```sql
mysqld --debug=S:T:t:r:p:n:L:i:F:f:D:d,info,error,query,qcache,my,exit,general,where:O,/tmp/mysqld.trace 
```

Or with Valgrind to get a complete stack trace on a crash.

```sql
valgrind /usr/local/mysql/bin/mysqld --basedir=/usr/local/mysql --datadir=/data/inetbase/mysql --plugin-dir=/usr/local/mysql/lib/plugin --user=mysql --log-error=/data/inetbase/mysql/lucifer.err --open-files-limit=65000 --pid-file=/var/run/mysqld/mysqld.pid --socket=/var/run/mysqld/mysqld.sock --port=3306
```

Report the issue in [MariaDB JIRA](https://jira.mariadb.org) (see [Reporting Bugs](/kb/en/reporting-bugs/)) or to the MariaDB Corporation support center.

## Compiling in Static

Available since version  3.1.14

To activate spider as a static plugin change "MODULE_ONLY" to "MANDATORY" in storage/spider/CMakeList.txt before compiling

Note that Spider UDF functions will not work with such settings.

## Status Variables

A number of new [status variables](/replication/optimization-and-tuning/system-variables/server-status-variables/) have been introduced:

- [Spider_direct_aggregate](/kb/en/spider-server-status-variables/#spider_direct_aggregate)
- [Spider_direct_order_limit](/kb/en/spider-server-status-variables/#spider_direct_order_limit)
- [Spider_mon_table_cache_version](/kb/en/spider-server-status-variables/#spider_mon_table_cache_version)
- [Spider_mon_table_cache_version_req](/kb/en/spider-server-status-variables/#spider_mon_table_cache_version_req)

## Information Schema Tables

- A new [Information Schema](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/) table is installed - [SPIDER_ALLOC_MEM](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-spider_alloc_mem-table/).

```sql
+-------------------+---------------------+------+-----+---------+-------+
| Field             | Type                | Null | Key | Default | Extra |
+-------------------+---------------------+------+-----+---------+-------+
| ID                | int(10) unsigned    | NO   |     | 0       |       |
| FUNC_NAME         | varchar(64)         | YES  |     | NULL    |       |
| FILE_NAME         | varchar(64)         | YES  |     | NULL    |       |
| LINE_NO           | int(10) unsigned    | YES  |     | NULL    |       |
| TOTAL_ALLOC_MEM   | bigint(20) unsigned | YES  |     | NULL    |       |
| CURRENT_ALLOC_MEM | bigint(20)          | YES  |     | NULL    |       |
| ALLOC_MEM_COUNT   | bigint(20) unsigned | YES  |     | NULL    |       |
| FREE_MEM_COUNT    | bigint(20) unsigned | YES  |     | NULL    |       |
+-------------------+---------------------+------+-----+---------+-------+
```

## Performance Schema

The [Performance schema](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/) is commonly used to troubleshoot issues that consume time inside your workload. The Performance schema should not be activated for servers that are experimenting constant heavy load, but most of time it is acceptable to lose 5% to 20% additional CPU to keep track of server internals execution.

To activate the performance schema, use the [performance_schema](/kb/en/performance-schema-system-variables/#performance_schema) system variable and add the following to the server section of the [MariaDB configuration file](/kb/en/configuring-mariadb-with-mycnf/).

```sql
performance_schema=on
```

Activate the Spider probes to be monitored.

```sql
UPDATE performance_schema.setup_instruments SET ENABLED='YES', TIMED='yes' WHERE NAME LIKE '%spider%';
```

Run your queries ...

And check the performance metrics. Remove specific Spider metrics to have a more global view.

```sql
SELECT * FROM performance_schema.events_waits_summary_global_by_event_name WHERE COUNT_STAR<>0 AND EVENT_NAME LIKE '%spider%' ORDER BY SUM_TIMER_WAIT DESC LIMIT 10;
```