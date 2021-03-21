# UUID_SHORT

## Syntax

```sql
UUID_SHORT()
```

## Description

Returns a "short" universal identifier as a 64-bit unsigned integer (rather
than a string-form 128-bit identifier as returned by the [UUID()](/built-in-functions/secondary-functions/miscellaneous-functions/uuid) function).

The value of <code class="highlight fixed" style="white-space:pre-wrap">UUID_SHORT()</code> is guaranteed to be unique if the
following conditions hold:

- The server_id of the current host is unique among your set of master and
  slave servers
- <code class="highlight fixed" style="white-space:pre-wrap">server_id</code> is between 0 and 255
- You don't set back your system time for your server between mysqld restarts
- You do not invoke <code class="highlight fixed" style="white-space:pre-wrap">UUID_SHORT()</code> on average more than 16
  million times per second between mysqld restarts

The UUID_SHORT() return value is constructed this way:

```sql
  (server_id & 255) << 56
+ (server_startup_time_in_seconds << 24)
+ incremented_variable++;
```

Statements using the UUID_SHORT() function are not [safe for statement-based replication](/kb/en/unsafe-statements-for-replication/).

## Examples

```sql
SELECT UUID_SHORT();
+-------------------+
| UUID_SHORT()      |
+-------------------+
| 21517162376069120 |
+-------------------+
```

```sql
create table t1 (a bigint unsigned default(uuid_short()) primary key);
insert into t1 values(),();
select * from t1;
+-------------------+
| a                 |
+-------------------+
| 98113699159474176 |
| 98113699159474177 |
+-------------------+
```

## See Also

- [UUID()](/built-in-functions/secondary-functions/miscellaneous-functions/uuid) ; Return full (128 bit) Universal Unique Identifier
- [AUTO_INCREMENT](/columns-storage-engines-and-plugins/data-types/auto_increment)
- [Sequences](/sql-statements-structure/sequences) - an alternative to auto_increment available from [MariaDB 10.3](/kb/en/what-is-mariadb-103/)