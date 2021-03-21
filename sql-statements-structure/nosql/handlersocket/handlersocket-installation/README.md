# HandlerSocket Installation

##### MariaDB starting with [5.3.0](/kb/en/mariadb-530-release-notes/)

Beginning with [MariaDB 5.3.0](/kb/en/mariadb-530-release-notes/), the [HandlerSocket](/sql-statements-structure/nosql/handlersocket/) plugin is
included in both source and binary distributions.

After MariaDB is installed,
use the [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin/) command (as the root user) to install
the HandlerSocket plugin. This command only needs to be run once, like so:

```sql
INSTALL PLUGIN handlersocket SONAME 'handlersocket.so';
```

In older versions, after installing the plugin, [SHOW PROCESSLIST](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-processlist/) would show
the HandlerSocket worker threads. With the latest versions, you first need to configure some settings. All [HandlerSocket configuration options](/sql-statements-structure/nosql/handlersocket/handlersocket-configuration-options/) are placed in the [mysqld] section of your my.cnf file.

At least the [handlersocket_address](/kb/en/handlersocket-configuration-options/#handlersocket_address), [handlersocket_port](/kb/en/handlersocket-configuration-options/#handlersocket_port) and [handlersocket_port_wr](/kb/en/handlersocket-configuration-options/#handlersocket_port_wr) options need to be set. For example:

```sql
handlersocket_address="127.0.0.1"
handlersocket_port="9998"
handlersocket_port_wr="9999"
```

After updating the configuration options, restart MariaDB.

On the client side, to make use of the plugin you will need to install the
appropriate client library (i.e. libhsclient for C++ applications and
perl-Net-HandlerSocket for perl applications, both available from the
[HandlerSocket website](https://github.com/ahiguti/HandlerSocket-Plugin-for-MySQL)).