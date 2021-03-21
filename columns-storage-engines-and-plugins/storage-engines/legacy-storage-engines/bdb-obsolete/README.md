# BDB

BDB was a storage engine included in old versions of MySQL.

This storage engine was removed from the 5.1 tree at some point, before the birth of MariaDB.

BDB permitted the use of a patched version of the Berkeley DB key-value store. It was a transactional storage engine. Locking was performed at page level.

## Limitations

- Indexes per table: 31
- Columns per index: 16
- Index size: 1024 bytes