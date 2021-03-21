# CONNECTION_ID

## Syntax

```sql
CONNECTION_ID()
```

## Description

Returns the connection ID (thread ID) for the connection. Every
thread (including events) has an ID that is unique among the set of currently
connected clients.

Until [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/), returns `MYSQL_TYPE_LONGLONG`, or [bigint(10)](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/bigint/), in all cases. From [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/), returns `MYSQL_TYPE_LONG`, or [int(10)](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/int/), when the result would fit within 32-bits.

## Examples

```sql
SELECT CONNECTION_ID();
+-----------------+
| CONNECTION_ID() |
+-----------------+
|               3 |
+-----------------+
```