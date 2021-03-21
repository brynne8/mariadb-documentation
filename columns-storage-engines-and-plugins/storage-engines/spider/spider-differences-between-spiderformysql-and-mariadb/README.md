# Spider Differences Between SpiderForMySQL and MariaDB

### SQL Syntax

- With `SpiderForMySQL`, the [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table) statement uses `CONNECTION` to define spider table variables whereas MariaDB uses `COMMENT`.

### Features

- [HANDLER](/sql-statements-structure/nosql/handler) can not be translated to SQL in MariaDB
- Concurrent background search is not yet implemented in MariaDB
- Vertical partitioning storage engine VP is not implemented in MariaDB
- `CREATE TABLE` can use [table discovery](/columns-storage-engines-and-plugins/storage-engines/storage-engines-storage-engine-development/table-discovery) in MariaDB
- <a undefined>JOIN</a> performance improvement using <a undefined>join_cache_level</a>&gt;1 and <a undefined>join_buffer_size</a> in MariaDB