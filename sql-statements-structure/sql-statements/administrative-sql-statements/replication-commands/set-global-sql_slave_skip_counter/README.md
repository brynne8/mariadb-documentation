# SET GLOBAL SQL_SLAVE_SKIP_COUNTER

## Syntax

```sql
SET GLOBAL sql_slave_skip_counter = N
```

## Description

This statement skips the next `<em>N</em>` events from the master. This is useful
for recovering from [replication](/replication/) stops caused by a statement.

If multi-source replication is used, this statement applies to the default connection. It could be necessary to change the value of the <a undefined>default_master_connection</a> server system variable.

Note that, if the event is a [transaction](/sql-statements-structure/sql-statements/transactions/), the whole transaction will be skipped. With non-transactional engines, an event is always a single statement.

This statement is valid only when the slave threads are not running.
Otherwise, it produces an error.

The statement does not automatically restart the slave threads.

## Example

```sql
SHOW SLAVE STATUS \G
...
SET GLOBAL sql_slave_skip_counter = 1;
START SLAVE;
```

Multi-source replication:

```sql
SET @@default_master_connection = 'master_01';
SET GLOBAL SQL_SLAVE_SKIP_COUNTER = 1;
START SLAVE;
```

## Multiple Replication Domains

`sql_slave_skip_counter` can't be used to skip transactions on a slave if [GTID replication](/replication/standard-replication/gtid/) is in use and if <a undefined>gtid_slave_pos</a> contains multiple <a undefined>gtid_domain_id</a> values. In that case, you'll get an error like the following:

```sql
ERROR 1966 (HY000): When using parallel replication and GTID with multiple 
 replication domains, @@sql_slave_skip_counter can not be used. Instead, 
 setting @@gtid_slave_pos explicitly can be  used to skip to after a given GTID 
 position.
```

In order to skip transactions in cases like this, you will have to manually change <a undefined>gtid_slave_pos</a>.

## See Also

- [Selectively Skipping Replication of Binlog Events](/replication/standard-replication/selectively-skipping-replication-of-binlog-events/)