# HandlerSocket

HandlerSocket gives you direct access to [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) and [SPIDER](/columns-storage-engines-and-plugins/storage-engines/spider/). It is included in MariaDB as a ready-to use plugin.

HandlerSocket is a NoSQL plugin for MariaDB. It works as a daemon inside the mysqld process, accepting TCP connections, and executing requests from clients. HandlerSocket does not support SQL queries. Instead, it supports simple CRUD operations on tables.

HandlerSocket can be much faster than mysqld/libmysql in some cases because it has lower CPU, disk, and network overhead:

1 To lower CPU usage it does not parse SQL.
2 Next, it batch-processes requests where possible, which further reduces CPU usage and lowers disk usage.
3 Lastly, the client/server protocol is very compact compared to mysql/libmysql, which reduces network usage.

- [HandlerSocket Installation](/sql-statements-structure/nosql/handlersocket/handlersocket-installation/) — Installing the HandlerSocket plugin
- [HandlerSocket Configuration Options](/sql-statements-structure/nosql/handlersocket/handlersocket-configuration-options/) — HandlerSocket configuration options.
- [HandlerSocket Client Libraries](/sql-statements-structure/nosql/handlersocket/handlersocket-client-libraries/) — Available HandlerSocket Client Libraries
- [Testing HandlerSocket in a Source Distribution](/sql-statements-structure/nosql/handlersocket/testing-handlersocket-in-a-source-distribution/) — Testing HandlerSocket in a source distribution
- [HandlerSocket External Resources](/sql-statements-structure/nosql/handlersocket/handlersocket-external-resources/) — HandlerSocket external resources and documentation