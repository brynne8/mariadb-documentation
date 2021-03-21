# Server Status Variables

The full list of status variables are listed in the contents on this page; most are described on this page, but some are described elsewhere:

- [Aria Status Variables](/kb/en/aria-server-status-variables/)
- [Galera Status Variables](/kb/en/mariadb-galera-cluster-status-variables/)
- [InnoDB Status Variables](/replication/optimization-and-tuning/system-variables/innodb-status-variables)
- [Mroonga Status Variables](/columns-storage-engines-and-plugins/storage-engines/mroonga/mroonga-status-variables)
- [MyRocks Status Variables](/columns-storage-engines-and-plugins/storage-engines/myrocks/myrocks-status-variables)
- [Performance Scheme Status Variables](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-status-variables)
- [Replication and Binary Log Status Variables](/kb/en/replication-and-binary-log-server-status-variables/)
- [S3 Storage Engine Status Variables](/columns-storage-engines-and-plugins/storage-engines/s3-storage-engine/s3-storage-engine-status-variables)
- [Server_Audit Status Variables](/kb/en/server_audit-status-variables/)
- [Sphinx Status Variables](/replication/optimization-and-tuning/system-variables/sphinx-status-variables)
- [Spider Status Variables](/kb/en/spider-server-status-variables/)
- [TokuDB Status Variables](/columns-storage-engines-and-plugins/storage-engines/tokudb/tokudb-status-variables)

See also the [Full list of MariaDB options, system and status variables](/mariadb-administration/variables-and-modes/full-list-of-mariadb-options-system-and-status-variables).

Use the [SHOW STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-status) statement to view status variables. This
information also can be obtained using the [mysqladmin extended-status](/clients-utilities/mysqladmin) command, or by querying the [Information Schema GLOBAL_STATUS and SESSION_STATUS](/kb/en/information-schema-global_status-and-session_status-tables/) tables.

Issuing a [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush) will reset many status variables to zero.

## List of Server Status Variables

#### `Aborted_clients`

- <strong>Description:</strong> Number of aborted client connections. This can be due to the client not calling mysql_close() before exiting, the client sleeping without issuing a request to the server for more seconds than specified by [wait_timeout](/kb/en/server-system-variables/#wait_timeout) or [interactive_timeout](/kb/en/server-system-variables/#interactive_timeout), or by the client program ending in the midst of transferring data. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Aborted_connects`

- <strong>Description:</strong> Number of failed server connection attempts. This can be due to a client using an incorrect password, a client not having privileges to connect to a database, a connection packet not containing the correct information, or if it takes more than [connect_timeout](/kb/en/server-system-variables/#connect_timeout) seconds to get a connect packet. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Aborted_connects_preauth`

- <strong>Description:</strong> Number of connection attempts that were aborted prior to authentication (regardless of whether or not an error occured).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.4.5](/kb/en/mariadb-1045-release-notes/)

---

#### `Access_denied_errors`

- <strong>Description:</strong> Number of access denied errors.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.2](/kb/en/what-is-mariadb-52/)

---

#### `Acl_column_grants`

- <strong>Description:</strong> Number of column permissions granted (rows in the [mysql.columns_priv table](/kb/en/mysqlcolumns_priv-table/)).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong>[MariaDB 10.1.4](/kb/en/mariadb-1014-release-notes/)

---

#### `Acl_database_grants`

- <strong>Description:</strong> Number of database permissions granted (rows in the [mysql.db table](/kb/en/mysqldb-table/)).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.4](/kb/en/mariadb-1014-release-notes/)

---

#### `Acl_function_grants`

- <strong>Description:</strong> Number of function permissions granted (rows in the [mysql.procs_priv table](/kb/en/mysqlprocs_priv-table/) with a routine type of `FUNCTION`).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.4](/kb/en/mariadb-1014-release-notes/)

---

#### `Acl_package_body_grants`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.3.5](/kb/en/mariadb-1035-release-notes/)

---

#### `Acl_package_spec_grants`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.3.5](/kb/en/mariadb-1035-release-notes/)

---

#### `Acl_procedure_grants`

- <strong>Description:</strong> Number of procedure permissions granted (rows in the [mysql.procs_priv table](/kb/en/mysqlprocs_priv-table/) with a routine type of `PROCEDURE`).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.4](/kb/en/mariadb-1014-release-notes/)

---

#### `Acl_proxy_users`

- <strong>Description:</strong> Number of proxy permissions granted (rows in the [mysql.proxies_priv table](/kb/en/mysqlproxies_priv-table/)).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.4](/kb/en/mariadb-1014-release-notes/)

---

#### `Acl_role_grants`

- <strong>Description:</strong> Number of role permissions granted (rows in the [mysql.roles_mapping table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlroles_mapping-table)).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.4](/kb/en/mariadb-1014-release-notes/)

---

#### `Acl_roles`

- <strong>Description:</strong> Number of roles (rows in the [mysql.user table](/kb/en/mysqluser-table/) where `is_role='Y'`).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.4](/kb/en/mariadb-1014-release-notes/)

---

#### `Acl_table_grants`

- <strong>Description:</strong> Number of table permissions granted (rows in the [mysql.tables_priv table](mysql.tables_priv-table)).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.4](/kb/en/mariadb-1014-release-notes/)

---

#### `Acl_users`

- <strong>Description:</strong> Number of users (rows in the [mysql.user table](/kb/en/mysqluser-table/) where `is_role='N'`).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.4](/kb/en/mariadb-1014-release-notes/)

---

#### `Busy_time`

- <strong>Description:</strong> Cumulative time in seconds of activity on connections.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.3](/kb/en/what-is-mariadb-53/)

---

#### `Bytes_received`

- <strong>Description:</strong> Total bytes received from all clients.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Bytes_sent`

- <strong>Description:</strong> Total bytes sent to all clients.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_admin_commands`

- <strong>Description:</strong> Number of admin commands executed. These include table dumps, change users, binary log dumps, shutdowns, pings and debugs.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_alter_db`

- <strong>Description:</strong> Number of [ALTER DATABASE](/sql-statements-structure/sql-statements/data-definition/alter/alter-database) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_alter_db_upgrade`

- <strong>Description:</strong> Number of [ALTER DATABASE ... UPGRADE](/sql-statements-structure/sql-statements/data-definition/alter/alter-database) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_alter_event`

- <strong>Description:</strong> Number of [ALTER EVENT](/programming-customizing-mariadb/triggers-events/event-scheduler/alter-event) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_alter_function`

- <strong>Description:</strong> Number of [ALTER FUNCTION](/sql-statements-structure/sql-statements/data-definition/alter/alter-function) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_alter_procedure`

- <strong>Description:</strong> Number of [ALTER PROCEDURE](/programming-customizing-mariadb/stored-routines/stored-procedures/alter-procedure) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_alter_sequence`

- <strong>Description:</strong> Number of [ALTER SEQUENCE](/sql-statements-structure/sequences/alter-sequence) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/)

---

#### `Com_alter_server`

- <strong>Description:</strong> Number of [ALTER SERVER](/sql-statements-structure/sql-statements/data-definition/alter/alter-server) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_alter_table`

- <strong>Description:</strong> Number of [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_alter_tablespace`

- <strong>Description:</strong> Number of [ALTER TABLESPACE](/sql-statements-structure/sql-statements/data-definition/alter/alter-tablespace) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_alter_user`

- <strong>Description:</strong> Number of [ALTER USER](/sql-statements-structure/sql-statements/account-management-sql-commands/alter-user) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.0](/kb/en/mariadb-1020-release-notes/)

---

#### `Com_analyze`

- <strong>Description:</strong> Number of [ANALYZE](/sql-statements-structure/sql-statements/table-statements/analyze-table) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_assign_to_keycache`

- <strong>Description:</strong> Number of assign to keycache commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_backup`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Removed:</strong> [MariaDB 10.4.1](/kb/en/mariadb-1041-release-notes/)

---

#### `Com_backup_lock`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Removed:</strong> [MariaDB 10.4.2](/kb/en/mariadb-1042-release-notes/)

---

#### `Com_backup_table`

- <strong>Description:</strong> Removed in [MariaDB 5.5](/kb/en/what-is-mariadb-55/). In older versions, Com_backup_table contains the number of [BACKUP TABLE](/kb/en/backup-table-deprecated/) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Removed:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/)

---

#### `Com_begin`

- <strong>Description:</strong> Number of [BEGIN](/programming-customizing-mariadb/programmatic-compound-statements/begin-end) statements executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_binlog`

- <strong>Description:</strong> Number of [BINLOG](/sql-statements-structure/sql-statements/administrative-sql-statements/binlog) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_call_procedure`

- <strong>Description:</strong> Number of [CALL](/sql-statements-structure/sql-statements/stored-routine-statements/call) procedure_name statements executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_change_db`

- <strong>Description:</strong> Number of [USE](/sql-statements-structure/sql-statements/administrative-sql-statements/use) database_name commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_check`

- <strong>Description:</strong> Number of [CHECK TABLE](/sql-statements-structure/sql-statements/table-statements/check-table) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_checksum`

- <strong>Description:</strong> Number of [CHECKSUM TABLE](/sql-statements-structure/sql-statements/table-statements/checksum-table) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_commit`

- <strong>Description:</strong> Number of [COMMIT](/kb/en/transactions-commit-statement/) commands executed. Differs from [Handler_commit](#handler_commit), which counts internal commit statements.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_compound_sql`

- <strong>Description:</strong> Number of [compund](/kb/en/programmatic-and-compound-statements/) sql statements.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/)

---

#### `Com_create_db`

- <strong>Description:</strong> Number of [CREATE DATABASE](/sql-statements-structure/sql-statements/data-definition/create/create-database) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_create_event`

- <strong>Description:</strong> Number of [CREATE EVENT](/sql-statements-structure/sql-statements/data-definition/create/create-event) commands executed. Differs from [Executed_events](#executed_events) in that it is incremented when the CREATE EVENT is run, and not when the event executes.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_create_function`

- <strong>Description:</strong> Number of [CREATE FUNCTION](/sql-statements-structure/sql-statements/data-definition/create/create-function) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_create_index`

- <strong>Description:</strong> Number of [CREATE INDEX](/sql-statements-structure/sql-statements/data-definition/create/create-index) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_create_package`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.3.5](/kb/en/mariadb-1035-release-notes/)

---

#### `Com_create_package_body`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.3.5](/kb/en/mariadb-1035-release-notes/)

---

#### `Com_create_procedure`

- <strong>Description:</strong> Number of [CREATE PROCEDURE](/programming-customizing-mariadb/stored-routines/stored-procedures/create-procedure) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_create_role`

- <strong>Description:</strong> Number of [CREATE ROLE](/sql-statements-structure/sql-statements/account-management-sql-commands/create-role) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/)

---

#### `Com_create_sequence`

- <strong>Description:</strong> Number of [CREATE SEQUENCE](/sql-statements-structure/sequences/create-sequence) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.3.0](/kb/en/mariadb-1030-release-notes/)

---

#### `Com_create_server`

- <strong>Description:</strong> Number of [CREATE SERVER](/sql-statements-structure/sql-statements/data-definition/create/create-server) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_create_table`

- <strong>Description:</strong> Number of [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_create_temporary_table`

- <strong>Description:</strong> Number of [CREATE TEMPORARY TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.6](/kb/en/mariadb-1016-release-notes/)

---

#### `Com_create_trigger`

- <strong>Description:</strong> Number of [CREATE TRIGGER](/programming-customizing-mariadb/triggers-events/triggers/create-trigger) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_create_udf`

- <strong>Description:</strong> Number of [CREATE UDF](/programming-customizing-mariadb/user-defined-functions/create-function-udf) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_create_user`

- <strong>Description:</strong> Number of [CREATE USER](/sql-statements-structure/sql-statements/account-management-sql-commands/create-user) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_create_view`

- <strong>Description:</strong> Number of [CREATE VIEW](/programming-customizing-mariadb/views/create-view) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_dealloc_sql`

- <strong>Description:</strong> Number of [DEALLOCATE](/kb/en/deallocate-drop-prepared-statement/) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_delete`

- <strong>Description:</strong> Number of [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete) commands executed. Differs from [Handler_delete](#handler_delete), which counts the number of times rows have been deleted from tables.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_delete_multi`

- <strong>Description:</strong> Number of multi-table [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_do`

- <strong>Description:</strong> Number of [DO](/sql-statements-structure/sql-statements/stored-routine-statements/do) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_drop_db`

- <strong>Description:</strong> Number of [DROP DATABASE](/sql-statements-structure/sql-statements/data-definition/drop/drop-database) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_drop_event`

- <strong>Description:</strong> Number of [DROP EVENT](/sql-statements-structure/sql-statements/data-definition/drop/drop-event) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_drop_function`

- <strong>Description:</strong> Number of [DROP FUNCTION](/programming-customizing-mariadb/stored-routines/stored-functions/drop-function) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_drop_index`

- <strong>Description:</strong> Number of [DROP INDEX](/sql-statements-structure/sql-statements/data-definition/drop/drop-index) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_drop_package`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.3.5](/kb/en/mariadb-1035-release-notes/)

---

#### `Com_drop_package_body`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.3.5](/kb/en/mariadb-1035-release-notes/)

---

#### `Com_drop_procedure`

- <strong>Description:</strong> Number of [DROP PROCEDURE](/programming-customizing-mariadb/stored-routines/stored-procedures/drop-procedure) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_drop_role`

- <strong>Description:</strong> Number of [DROP ROLE](/sql-statements-structure/sql-statements/account-management-sql-commands/drop-role) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/)

---

#### `Com_drop_sequence`

- <strong>Description:</strong> Number of [DROP SEQUENCE](/sql-statements-structure/sequences/drop-sequence) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.3.0](/kb/en/mariadb-1030-release-notes/)

---

#### `Com_drop_server`

- <strong>Description:</strong> Number of [DROP SERVER](/sql-statements-structure/sql-statements/data-definition/drop/drop-server) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_drop_table`

- <strong>Description:</strong> Number of [DROP TABLE](/sql-statements-structure/sql-statements/data-definition/drop/drop-table) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_drop_temporary_table`

- <strong>Description:</strong> Number of [DROP TEMPORARY TABLE](/sql-statements-structure/sql-statements/data-definition/drop/drop-table) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.6](/kb/en/mariadb-1016-release-notes/)

---

#### `Com_drop_trigger`

- <strong>Description:</strong> Number of [DROP TRIGGER](/sql-statements-structure/sql-statements/data-definition/drop/drop-trigger) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_drop_user`

- <strong>Description:</strong> Number of [DROP USER](/sql-statements-structure/sql-statements/account-management-sql-commands/drop-user) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_drop_view`

- <strong>Description:</strong> Number of [DROP VIEW](/programming-customizing-mariadb/views/drop-view) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_empty_query`

- <strong>Description:</strong> Number of queries to the server that do not produce SQL queries. An SQL query simply returning no results does not increment `Com_empty_query` - see [Empty_queries](#empty_queries) instead. An example of an empty query sent to the server is <code class="fixed" style="white-space:pre-wrap">mysql --comments -e '-- sql comment' </code>
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_execute_immediate`

- <strong>Description:</strong> Number of [EXECUTE IMMEDIATE](/sql-statements-structure/sql-statements/prepared-statements/execute-immediate) statements executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/)

---

#### `Com_execute_sql`

- <strong>Description:</strong> Number of [EXECUTE](/sql-statements-structure/sql-statements/prepared-statements/execute-statement) statements executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_flush`

- <strong>Description:</strong> Number of [FLUSH](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush) commands executed. This differs from [Flush_commands](#flush_commands), which also counts internal server flush requests.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_get_diagnostics`

- <strong>Description:</strong> Number of [GET DIAGNOSTICS](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-diagnostics/get-diagnostics) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.0.4](/kb/en/mariadb-1004-release-notes/)

---

#### `Com_grant`

- <strong>Description:</strong> Number of [GRANT](/sql-statements-structure/sql-statements/account-management-sql-commands/grant) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_grant_role`

- <strong>Description:</strong> Number of [GRANT](/kb/en/grant/#roles) role commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/)

---

#### `Com_ha_close`

- <strong>Description:</strong> Number of [HANDLER](/sql-statements-structure/nosql/handler/handler-commands) table_name CLOSE commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_ha_open`

- <strong>Description:</strong> Number of [HANDLER](/sql-statements-structure/nosql/handler/handler-commands) table_name OPEN commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_ha_read`

- <strong>Description:</strong> Number of [HANDLER](/sql-statements-structure/nosql/handler/handler-commands) table_name READ commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_help`

- <strong>Description:</strong> Number of [HELP](/sql-statements-structure/sql-statements/administrative-sql-statements/help-command) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_insert`

- <strong>Description:</strong> Number of [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_insert_select`

- <strong>Description:</strong> Number of [INSERT ... SELECT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-select) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_install_plugin`

- <strong>Description:</strong> Number of [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_kill`

- <strong>Description:</strong> Number of [KILL](/sql-statements-structure/sql-statements/administrative-sql-statements/kill) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_load`

- <strong>Description:</strong> Number of LOAD commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_load_master_data`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Removed:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/)

---

#### `Com_load_master_table`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Removed:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/)

---

#### `Com_multi`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.0](/kb/en/mariadb-1020-release-notes/)

---

#### `Com_lock_tables`

- <strong>Description:</strong> Number of [lock-tables|LOCK TABLES]] commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_optimize`

- <strong>Description:</strong> Number of [OPTIMIZE](/replication/optimization-and-tuning/optimizing-tables/optimize-table) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_preload_keys`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_prepare_sql`

- <strong>Description:</strong> Number of [PREPARE](/sql-statements-structure/sql-statements/prepared-statements/prepare-statement) statements executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_purge`

- <strong>Description:</strong> Number of [PURGE](/kb/en/sql-commands-purge-logs/) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_purge_before_date`

- <strong>Description:</strong> Number of [PURGE BEFORE](/kb/en/sql-commands-purge-logs/) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_release_savepoint`

- <strong>Description:</strong> Number of [RELEASE SAVEPOINT](/sql-statements-structure/sql-statements/transactions/savepoint) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_rename_table`

- <strong>Description:</strong> Number of [RENAME TABLE](/sql-statements-structure/sql-statements/data-definition/rename-table) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_rename_user`

- <strong>Description:</strong> Number of [RENAME USER](/sql-statements-structure/sql-statements/account-management-sql-commands/rename-user) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_repair`

- <strong>Description:</strong> Number of [REPAIR TABLE](/sql-statements-structure/sql-statements/table-statements/repair-table) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_replace`

- <strong>Description:</strong> Number of [REPLACE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/replace) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_replace_select`

- <strong>Description:</strong> Number of [REPLACE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/replace) ... [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_reset`

- <strong>Description:</strong> Number of [RESET](/sql-statements-structure/sql-statements/administrative-sql-statements/reset) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_resignal`

- <strong>Description:</strong> Number of [RESIGNAL](/kb/en/resignal-statement/) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_restore_table`

- <strong>Description:</strong> Removed in [MariaDB 5.5](/kb/en/what-is-mariadb-55/). In older versions, Com_restore_table contains the number of [RESTORE TABLE](/sql-statements-structure/sql-statements/table-statements/obsolete-table-commands/restore-table-removed) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Removed:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/)

---

#### `Com_revoke`

- <strong>Description:</strong> Number of [REVOKE](/sql-statements-structure/sql-statements/account-management-sql-commands/revoke) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_revoke_all`

- <strong>Description:</strong> Number of [REVOKE ALL](/sql-statements-structure/sql-statements/account-management-sql-commands/revoke) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_revoke_grant`

- <strong>Description:</strong> Number of [REVOKE](/kb/en/revoke/#roles) role commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/)

---

#### `Com_rollback`

- <strong>Description:</strong> Number of [ROLLBACK](/sql-statements-structure/sql-statements/transactions/rollback) commands executed. Differs from [Handler_rollback](#handler_rollback), which is the number of transaction rollback requests given to a storage engine.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_rollback_to_savepoint`

- <strong>Description:</strong> Number of [ROLLBACK ... TO SAVEPOINT](/sql-statements-structure/sql-statements/transactions/rollback) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_savepoint`

- <strong>Description:</strong> Number of [SAVEPOINT](/sql-statements-structure/sql-statements/transactions/savepoint) commands executed. Differs from [Handler_savepoint](#handler_savepoint), which is the number of transaction savepoint creation requests.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_select`

- <strong>Description:</strong> Number of [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select) commands executed. Also includes queries that make use of the [query cache](/replication/optimization-and-tuning/buffers-caches-and-threads/query-cache).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_set_option`

- <strong>Description:</strong> Number of [SET OPTION](/sql-statements-structure/sql-statements/administrative-sql-statements/set-commands/set) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_signal`

- <strong>Description:</strong> Number of [SIGNAL](/kb/en/signal-statement/) statements executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_show_authors`

- <strong>Description:</strong> Number of [SHOW AUTHORS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-authors) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_show_binlog_events`

- <strong>Description:</strong> Number of [SHOW BINLOG EVENTS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-binlog-events) statements executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_show_binlogs`

- <strong>Description:</strong> Number of [SHOW BINARY LOGS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-binary-logs) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_show_charsets`

- <strong>Description:</strong> Number of [SHOW CHARACTER SET](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-character-set) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_show_client_statistics`

- <strong>Description:</strong> Number of [SHOW CLIENT STATISTICS](/kb/en/show-client_statistics/) commands executed. Removed in [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/) when that statement was replaced by the generic [SHOW information_schema_table](/kb/en/information-schema-plugins-show-and-flush-statements/).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.2](/kb/en/what-is-mariadb-52/)
- <strong>Removed:</strong> [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/)

---

#### `Com_show_collations`

- <strong>Description:</strong> Number of [SHOW COLLATION](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-collation) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_show_column_types`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Removed:</strong> [MariaDB 5.5](/kb/en/what-is-mariadb-55/)

---

#### `Com_show_contributors`

- <strong>Description:</strong> Number of [SHOW CONTRIBUTORS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-contributors) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_show_create_db`

- <strong>Description:</strong> Number of [SHOW CREATE DATABASE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-database) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_show_create_event`

- <strong>Description:</strong> Number of [SHOW CREATE EVENT](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-event) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_show_create_func`

- <strong>Description:</strong> Number of [SHOW CREATE FUNCTION](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-function) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_show_create_package`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.3.5](/kb/en/mariadb-1035-release-notes/)

---

#### `Com_show_create_package_body`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.3.5](/kb/en/mariadb-1035-release-notes/)

---

#### `Com_show_create_proc`

- <strong>Description:</strong> Number of [SHOW CREATE PROCEDURE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-procedure) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_show_create_table`

- <strong>Description:</strong> Number of [SHOW CREATE TABLE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-table) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_show_create_trigger`

- <strong>Description:</strong> Number of [SHOW CREATE TRIGGER](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-table) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_show_create_user`

- <strong>Description:</strong> Number of [SHOW CREATE USER](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-user) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.0](/kb/en/mariadb-1020-release-notes/)

---

#### `Com_show_databases`

- <strong>Description:</strong> Number of [SHOW DATABASES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-databases) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_show_engine_logs`

- <strong>Description:</strong> Number of [SHOW ENGINE LOGS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_show_engine_mutex`

- <strong>Description:</strong> Number of [SHOW ENGINE MUTEX](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_show_engine_status`

- <strong>Description:</strong> Number of [SHOW ENGINE STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_show_events`

- <strong>Description:</strong> Number of [SHOW EVENTS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-events) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_show_errors`

- <strong>Description:</strong> Number of [SHOW ERRORS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-errors) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_show_explain`

- <strong>Description:</strong> Number of [SHOW EXPLAIN](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-explain) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `Com_show_fields`

- <strong>Description:</strong> Number of [SHOW COLUMNS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-columns) or SHOW FIELDS commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_show_function_status`

- <strong>Description:</strong> Number of [SHOW FUNCTION STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-function-status) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_show_generic`

- <strong>Description:</strong> Number of generic [SHOW](/sql-statements-structure/sql-statements/administrative-sql-statements/show) commands executed, such as [SHOW INDEX_STATISTICS](/kb/en/show-index_statistics/) and [SHOW TABLE_STATISTICS](/kb/en/show-table_statistics/)
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/)

---

#### `Com_show_grants`

- <strong>Description:</strong> Number of [SHOW GRANTS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-grants) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_show_keys`

- <strong>Description:</strong> Number of [SHOW INDEX](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-index) or SHOW KEYS commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_show_index_statistics`

- <strong>Description:</strong> Number of [SHOW INDEX_STATISTICS](/kb/en/show-index_statistics/) commands executed. Removed in [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/) when that statement was replaced by the generic [SHOW information_schema_table](/kb/en/information-schema-plugins-show-and-flush-statements/).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.2](/kb/en/what-is-mariadb-52/)
- <strong>Removed:</strong> [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/)

---

#### `Com_show_open_tables`

- <strong>Description:</strong> Number of [SHOW OPEN TABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-open-tables) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_show_package_status`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.3.5](/kb/en/mariadb-1035-release-notes/)

---

#### `Com_show_package_body_status`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.3.5](/kb/en/mariadb-1035-release-notes/)

---

#### `Com_show_plugins`

- <strong>Description:</strong> Number of [SHOW PLUGINS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-plugins) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_show_privileges`

- <strong>Description:</strong> Number of [SHOW PRIVILEGES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-privileges) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_show_procedure_status`

- <strong>Description:</strong> Number of [SHOW PROCEDURE STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-procedure-status) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_show_processlist`

- <strong>Description:</strong> Number of [SHOW PROCESSLIST](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-processlist) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_show_profile`

- <strong>Description:</strong> Number of [SHOW PROFILE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-profile) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_show_profiles`

- <strong>Description:</strong> Number of [SHOW PROFILES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-profiles) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_show_relaylog_events`

- <strong>Description:</strong> Number of [SHOW RELAYLOG EVENTS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-relaylog-events) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_show_status`

- <strong>Description:</strong> Number of [SHOW STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-status) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`st

---

#### `Com_show_storage_engines`

- <strong>Description:</strong> Number of [SHOW STORAGE ENGINES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engines) - or `SHOW ENGINES` -  commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_show_table_statistics`

- <strong>Description:</strong> Number of [SHOW TABLE STATISTICS](/kb/en/show-table_statistics/) commands executed. Removed in [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/) when that statement was replaced by the generic [SHOW information_schema_table](/kb/en/information-schema-plugins-show-and-flush-statements/).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.2](/kb/en/what-is-mariadb-52/)
- <strong>Removed:</strong> [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/)

---

#### `Com_show_table_status`

- <strong>Description:</strong> Number of [SHOW TABLE STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-table-status) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_show_tables`

- <strong>Description:</strong> Number of [SHOW TABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-tables) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_show_triggers`

- <strong>Description:</strong> Number of [SHOW TRIGGERS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-triggers) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_show_user_statistics`

- <strong>Description:</strong> Number of [SHOW USER STATISTICS](/kb/en/show-user_statistics/) commands executed. Removed in [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/) when that statement was replaced by the generic [SHOW information_schema_table](/kb/en/information-schema-plugins-show-and-flush-statements/).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.2](/kb/en/what-is-mariadb-52/)
- <strong>Removed:</strong> [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/)

---

#### `Com_show_variable`

- <strong>Description:</strong> Number of [SHOW VARIABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-variables) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_show_warnings`

- <strong>Description:</strong> Number of [SHOW WARNINGS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-warnings) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_shutdown`

- <strong>Description:</strong> Number of [SHUTDOWN](/sql-statements-structure/sql-statements/administrative-sql-statements/shutdown) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_stmt_close`

- <strong>Description:</strong> Number of [prepared statements](/sql-statements-structure/sql-statements/prepared-statements) closed ([deallocated or dropped](/kb/en/deallocate-drop-prepared-statement/)).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_stmt_execute`

- <strong>Description:</strong> Number of [prepared statements](/sql-statements-structure/sql-statements/prepared-statements) [executed](/sql-statements-structure/sql-statements/prepared-statements/execute-statement).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_stmt_fetch`

- <strong>Description:</strong> Number of [prepared statements](/sql-statements-structure/sql-statements/prepared-statements) fetched.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_stmt_prepare`

- <strong>Description:</strong> Number of [prepared statements](/sql-statements-structure/sql-statements/prepared-statements) [prepared](/sql-statements-structure/sql-statements/prepared-statements/prepare-statement).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_stmt_reprepare`

- <strong>Description:</strong> Number of [prepared statements](/sql-statements-structure/sql-statements/prepared-statements) reprepared.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_stmt_reset`

- <strong>Description:</strong> Number of [prepared statements](/sql-statements-structure/sql-statements/prepared-statements) where the data of a prepared statement which was accumulated in chunks by sending long data has been reset.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_stmt_send_long_data`

- <strong>Description:</strong> Number of [prepared statements](/sql-statements-structure/sql-statements/prepared-statements) where the parameter data has been sent in chunks (long data).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_truncate`

- <strong>Description:</strong> Number of [TRUNCATE](/built-in-functions/numeric-functions/truncate) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_uninstall_plugin`

- <strong>Description:</strong> Number of [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_unlock_tables`

- <strong>Description:</strong> Number of [UNLOCK TABLES](/kb/en/transactions-lock/) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_update`

- <strong>Description:</strong> Number of [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_update_multi`

- <strong>Description:</strong> Number of multi-table [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_xa_commit`

- <strong>Description:</strong>  Number of XA statements committed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_xa_end`

- <strong>Description:</strong> Number of XA statements ended.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_xa_prepare`

- <strong>Description:</strong> Number of XA statements prepared.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_xa_recover`

- <strong>Description:</strong> Number of XA RECOVER statements executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_xa_rollback`

- <strong>Description:</strong> Number of XA statements rolled back.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Com_xa_start`

- <strong>Description:</strong> Number of XA statements started.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Compression`

- <strong>Description:</strong> Whether client-server traffic is compressed.
- <strong>Scope:</strong> Session
- <strong>Data Type:</strong> `boolean`

---

#### `Connection_errors_accept`

- <strong>Description:</strong> Number of errors that occurred during calls to accept() on the listening port. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.0.4](/kb/en/mariadb-1004-release-notes/)

---

#### `Connection_errors_internal`

- <strong>Description:</strong> Number of refused connections due to internal server errors, for example out of memory errors, or failed thread starts. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.0.4](/kb/en/mariadb-1004-release-notes/)

---

#### `Connection_errors_max_connections`

- <strong>Description:</strong> Number of refused connections due to the [max_connections](/kb/en/server-system-variables/#max_connections) limit being reached. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.0.4](/kb/en/mariadb-1004-release-notes/)

---

#### `Connection_errors_peer_address`

- <strong>Description:</strong> Number of errors while searching for the connecting client IP address. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.0.4](/kb/en/mariadb-1004-release-notes/)

---

#### `Connection_errors_select`

- <strong>Description:</strong> Number of errors during calls to select() or poll() on the listening port. The client would not necessarily have been rejected in these cases. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.0.4](/kb/en/mariadb-1004-release-notes/)

---

#### `Connection_errors_tcpwrap`

- <strong>Description:</strong> Number of connections the libwrap library refused. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.0.4](/kb/en/mariadb-1004-release-notes/)

---

#### `Connections`

- <strong>Description:</strong> Number of connection attempts (both successful and unsuccessful)
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Cpu_time`

- <strong>Description:</strong> Total CPU time used.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.3](/kb/en/what-is-mariadb-53/)

---

#### `Created_tmp_disk_tables`

- <strong>Description:</strong> Number of on-disk temporary tables created.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Created_tmp_files`

- <strong>Description:</strong> Number of temporary files created. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Created_tmp_tables`

- <strong>Description:</strong> Number of in-memory temporary tables created.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Delayed_errors`

- <strong>Description:</strong> Number of errors which occurred while doing [INSERT DELAYED](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-delayed). The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Delayed_insert_threads`

- <strong>Description:</strong> Number of [INSERT DELAYED](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-delayed) threads.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Delayed_writes`

- <strong>Description:</strong> Number of [INSERT DELAYED](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-delayed) rows written. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Delete_scan`

- <strong>Description:</strong> Number of [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete)s that required a full table scan.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.17](/kb/en/mariadb-10117-release-notes/), [MariaDB 10.0.27](/kb/en/mariadb-10027-release-notes/)

---

#### `Empty_queries`

- <strong>Description:</strong> Number of queries returning no results. Note this is not the same as [Com_empty_query](#com_empty_query).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.2](/kb/en/what-is-mariadb-52/)

---

#### `Executed_events`

- <strong>Description:</strong> Number of times events created with [CREATE EVENT](/sql-statements-structure/sql-statements/data-definition/create/create-event) have executed. This differs from [Com_create_event](#com_create_event) in that it is only incremented when the event has run, not when it executes.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Executed_triggers`

- <strong>Description:</strong> Number of times triggers created with [CREATE TRIGGER](/programming-customizing-mariadb/triggers-events/triggers/create-trigger) have executed. This differs from [Com_create_trigger](#com_create_trigger) in that it is only incremented when the trigger has run, not when it executes.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Feature_application_time_periods`

- <strong>Description:</strong> Number of times a table created with [periods](/kb/en/create-table/#periods) has been opened.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.4.5](/kb/en/mariadb-1045-release-notes/)

---

#### `Feature_check_constraint`

- <strong>Description:</strong> Number of times [constraints](/sql-statements-structure/sql-statements/data-definition/constraint) were checked. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/)

---

#### `Feature_custom_aggregate_functions`

- <strong>Description:</strong> Number of queries which make use of [custom aggregate functions](stored-aggregate-function).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.3.6](/kb/en/mariadb-1036-release-notes/)

---

#### `Feature_delay_key_write`

- <strong>Description:</strong> Number of tables opened that are using [delay_key_write](/kb/en/server-system-variables/#delay_key_write). The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.0.13](/kb/en/mariadb-10013-release-notes/)

---

#### `Feature_dynamic_columns`

- <strong>Description:</strong> Number of times the [COLUMN_CREATE()](/built-in-functions/special-functions/dynamic-columns-functions/column_create) function was used.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Feature_fulltext`

- <strong>Description:</strong> Number of times the [MATCH … AGAINST()](/built-in-functions/string-functions/match-against) function was used.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Feature_gis`

- <strong>Description:</strong> Number of times a table with a any of the [geometry](/sql-statements-structure/geographic-geometric-features/geometry-types) columns was opened.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Feature_insert_returning`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/)

---

#### `Feature_invisible_columns`

- <strong>Description:</strong> Number of invisible columns in all opened tables.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/)

---

#### `Feature_json`

- <strong>Description:</strong> Number of times JSON functionality has been used, such as one of the [JSON functions](/built-in-functions/special-functions/json-functions). Does not include the [CONNECT engine JSON type](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-json-table-type), or [EXPLAIN/ANALYZE FORMAT=JSON](/kb/en/analyze-statement/#analyze-formatjson).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/)

---

#### `Feature_locale`

- <strong>Description:</strong> Number of times the [@@lc_messages](/kb/en/server-system-variables/#lc_messages) variable was assigned into.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Feature_subquery`

- <strong>Description:</strong> Number of subqueries (excluding subqueries in the FROM clause) used.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Feature_system_versioning`

- <strong>Description:</strong> Number of times [system versioning](/sql-statements-structure/temporal-tables/system-versioned-tables) functionality has been used (opening a table WITH SYSTEM VERSIONING).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/)

#### `Feature_timezone`

- <strong>Description:</strong> Number of times an explicit timezone (excluding [UTC](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/coordinated-universal-time) and SYSTEM) was specified.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Feature_trigger`

- <strong>Description:</strong> Number of triggers loaded.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Feature_window_functions`

- <strong>Description:</strong> Number of times [window functions](/built-in-functions/special-functions/window-functions) were used.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/)

---

#### `Feature_xml`

- <strong>Description:</strong> Number of times XML functions ([EXTRACTVALUE()](/built-in-functions/string-functions/extractvalue) and [UPDATEXML()](/built-in-functions/string-functions/updatexml)) were used.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Flush_commands`

- <strong>Description:</strong> Number of [FLUSH](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush) statements executed, as well as due to internal server flush requests. This differs from [Com_flush](#com_flush), which simply counts FLUSH statements, not internal server flush operations.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Removed:</strong> [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/)

---

#### `Handler_commit`

- <strong>Description:</strong> Number of internal [COMMIT](/sql-statements-structure/sql-statements/transactions/commit) requests. Differs from [Com_commit](#com_commit), which counts the number of [COMMIT](/sql-statements-structure/sql-statements/transactions/commit) statements executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Handler_delete`

- <strong>Description:</strong> Number of times rows have been deleted from tables. Differs from [Com_delete](#com_delete), which counts [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete) statements.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Handler_discover`

- <strong>Description:</strong> Discovery is when the server asks the NDBCLUSTER storage engine if it knows about a table with a given name. Handler_discover indicates the number of times that tables have been discovered in this way.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Handler_external_lock`

- <strong>Description:</strong> Incremented for each call to the external_lock() function, which generally occurs at the beginning and end of access to a table instance.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

---

#### `Handler_icp_attempts`

- <strong>Description:</strong> Number of times pushed index condition was checked. The smaller the ratio of Handler_icp_attempts to [Handler_icp_match](#handler_icp_match) the better the filtering. See [Index Condition Pushdown](/replication/optimization-and-tuning/query-optimizations/index-condition-pushdown).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.3](/kb/en/what-is-mariadb-53/)

---

#### `Handler_icp_match`

- <strong>Description:</strong> Number of times pushed index condition was matched. The smaller the ratio of [Handler_icp_attempts](#handler_icp_attempts) to Handler_icp_match the better the filtering. See See [Index Condition Pushdown](/replication/optimization-and-tuning/query-optimizations/index-condition-pushdown).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.3](/kb/en/what-is-mariadb-53/)

---

#### `Handler_mrr_init`

- <strong>Description:</strong> Counts how many MRR (multi-range read) scans were performed. See [Multi Range Read optimization](/replication/optimization-and-tuning/mariadb-internal-optimizations/multi-range-read-optimization).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.3](/kb/en/what-is-mariadb-53/)

---

#### `Handler_mrr_key_refills`

- <strong>Description:</strong> Number of times key buffer was refilled (not counting the initial fill). A non-zero value indicates there wasn't enough memory to do key sort-and-sweep passes in one go.  See [Multi Range Read optimization](/replication/optimization-and-tuning/mariadb-internal-optimizations/multi-range-read-optimization).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.3](/kb/en/what-is-mariadb-53/)

---

#### `Handler_mrr_rowid_refills`

- <strong>Description:</strong> Number of times rowid buffer was refilled (not counting the initial fill). A non-zero value indicates there wasn't enough memory to do rowid sort-and-sweep passes in one go. See [Multi Range Read optimization](/replication/optimization-and-tuning/mariadb-internal-optimizations/multi-range-read-optimization).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.3](/kb/en/what-is-mariadb-53/)

---

#### `Handler_prepare`

- <strong>Description:</strong> Number of two-phase commit prepares.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Handler_read_first`

- <strong>Description:</strong> Number of requests to read the first row from an index. A high value indicates many full index scans, e.g. <code class="fixed" style="white-space:pre-wrap">SELECT a FROM table_name</code> where `a` is an indexed column.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Handler_read_key`

- <strong>Description:</strong> Number of row read requests based on an index value. A high value indicates indexes are regularly being used, which is usually positive.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Handler_read_last`

- <strong>Description:</strong> Number of requests to read the last row from an index. [ORDER BY DESC](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/order-by) results in a last-key request followed by several previous-key requests.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Handler_read_next`

- <strong>Description:</strong> Number of requests to read the next row from an index (in order). Increments when doing an index scan or querying an index column with a range constraint.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Handler_read_prev`

- <strong>Description:</strong> Number of requests to read the previous row from an index (in order). Mostly used with [ORDER BY DESC](/kb/en/select/#order-by).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Handler_read_retry`

- <strong>Description:</strong> Number of read retrys triggered by semi_consistent_read (InnoDB feature).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.0.27](/kb/en/mariadb-10027-release-notes/)

---

#### `Handler_read_rnd`

- <strong>Description:</strong> Number of requests to read a row based on its position. If this value is high, you may not be using joins that don't use indexes properly, or be doing many full table scans.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Handler_read_rnd_deleted`

- <strong>Description:</strong> Number of requests to delete a row based on its position.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.3](/kb/en/what-is-mariadb-53/)

---

#### `Handler_read_rnd_next`

- <strong>Description:</strong> Number of requests to read the next row. A large number of these may indicate many table scans and improperly used indexes.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Handler_rollback`

- <strong>Description:</strong> Number of transaction rollback requests given to a storage engine. Differs from [Com_rollback](#com_rollback), which is the number of [ROLLBACK](/sql-statements-structure/sql-statements/transactions/rollback) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Handler_savepoint`

- <strong>Description:</strong> Number of transaction savepoint creation requests. Differs from [Com_savepoint](#com_savepoint) which is the number of [SAVEPOINT](/sql-statements-structure/sql-statements/transactions/savepoint) commands executed.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Handler_savepoint_rollback`

- <strong>Description:</strong> Number of requests to rollback to a transaction [savepoint](/sql-statements-structure/sql-statements/transactions/savepoint).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Handler_tmp_delete`

- <strong>Description:</strong> Number of requests to delete a row in a temporary table.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.3.0](/kb/en/mariadb-1030-release-notes/)

---

#### `Handler_tmp_update`

- <strong>Description:</strong> Number of requests to update a row to a temporary table.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.3](/kb/en/what-is-mariadb-53/)

---

#### `Handler_tmp_write`

- <strong>Description:</strong> Number of requests to write a row to a temporary table.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.3](/kb/en/what-is-mariadb-53/)

---

#### `Handler_update`

- <strong>Description:</strong> Number of requests to update a row in a table. Since [MariaDB 5.3](/kb/en/what-is-mariadb-53/), this no longer counts temporary tables - see [Handler_tmp_update](#handler_tmp_update).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Handler_write`

- <strong>Description:</strong> Number of requests to write a row to a table. Since [MariaDB 5.3](/kb/en/what-is-mariadb-53/), this no longer counts temporary tables - see [Handler_tmp_write](#handler_tmp_write).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Key_blocks_not_flushed`

- <strong>Description:</strong> Number of key cache blocks which have been modified but not flushed to disk.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Key_blocks_unused`

- <strong>Description:</strong> Number of unused key cache blocks.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Key_blocks_used`

- <strong>Description:</strong> Max number of key cache blocks which have been used simultaneously.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Key_blocks_warm`

- <strong>Description:</strong> Number of key cache blocks in the warm list.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.1](/kb/en/what-is-mariadb-51/)

---

#### `Key_read_requests`

- <strong>Description:</strong> Number of key cache block read requests. See [Optimizing key_buffer_size](/replication/optimization-and-tuning/system-variables/optimizing-key_buffer_size).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Key_reads`

- <strong>Description:</strong> Number of physical index block reads. See [Optimizing key_buffer_size](/replication/optimization-and-tuning/system-variables/optimizing-key_buffer_size).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Key_write_requests`

- <strong>Description:</strong> Number of requests to write a block to the key cache.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Key_writes`

- <strong>Description:</strong> Number of key cache block write requests
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Last_query_cost`

- <strong>Description:</strong> The most recent query optimizer query cost calculation. Can not be calculated for complex queries, such as subqueries or UNION. It will be set to 0 for complex queries.
- <strong>Scope:</strong> Session
- <strong>Data Type:</strong> `numeric`

---

#### `Maria_*`

- <strong>Description:</strong> When the Maria storage engine was renamed Aria, the Maria variables existing at the time were renamed at the same time. See [Aria Server Status Variables](/kb/en/aria-server-status-variables/).

---

#### `Max_statement_time_exceeded`

- <strong>Description:</strong> Number of queries that exceeded the execution time specified by [max_statement_time](/kb/en/server-system-variables/#max_statement_time). See [Aborting statements that take longer than a certain time to execute](/kb/en/aborting-statements-that-take-longer-than-a-certain-time-to-execute/).
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/)

---

#### `Max_used_connections`

- <strong>Description:</strong> Max number of connections ever open at the same time. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush).
- <strong>Scope:</strong> Session
- <strong>Data Type:</strong> `numeric`

---

#### `Memory_used`

- <strong>Description:</strong> Global or per-connection memory usage, in bytes. This includes all per-connection memory allocations, but excludes global allocations such as the key_buffer, innodb_buffer_pool etc.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Memory_used_initial`

- <strong>Description:</strong> Amount of memory that was used when the server started to service the user connections.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/)

---

#### `Not_flushed_delayed_rows`

- <strong>Description:</strong> Number of [INSERT DELAYED](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-delayed) rows waiting to be written.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Open_files`

- <strong>Description:</strong> Number of regular files currently opened by the server. Does not include sockets or pipes, or storage engines using internal functions.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Open_streams`

- <strong>Description:</strong> Number of currently opened streams, usually log files.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Open_table_definitions`

- <strong>Description:</strong> Number of currently cached .frm files.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Open_tables`

- <strong>Description:</strong> Number of currently opened tables, excluding temporary tables.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Opened_files`

- <strong>Description:</strong> Number of files the server has opened.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Opened_plugin_libraries`

- <strong>Description:</strong> Number of shared libraries that the server has opened to load [plugins](/columns-storage-engines-and-plugins/plugins).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

---

#### `Opened_table_definitions`

- <strong>Description:</strong> Number of .frm files that have been cached.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Opened_tables`

- <strong>Description:</strong> Number of tables the server has opened.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Opened_views`

- <strong>Description:</strong> Number of views the server has opened.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Prepared_stmt_count`

- <strong>Description:</strong> Current number of prepared statements.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Qcache_free_blocks`

- <strong>Description:</strong> Number of free [query cache](/replication/optimization-and-tuning/buffers-caches-and-threads/query-cache) memory blocks.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Qcache_free_memory`

- <strong>Description:</strong> Amount of free [query cache](/replication/optimization-and-tuning/buffers-caches-and-threads/query-cache) memory.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Qcache_hits`

- <strong>Description:</strong> Number of requests served by the  [query cache](/replication/optimization-and-tuning/buffers-caches-and-threads/query-cache). The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Qcache_inserts`

- <strong>Description:</strong> Number of queries ever cached in the  [query cache](/replication/optimization-and-tuning/buffers-caches-and-threads/query-cache). The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Qcache_lowmem_prunes`

- <strong>Description:</strong> Number of pruning operations performed to remove old results to make space for new results in the [query cache](/replication/optimization-and-tuning/buffers-caches-and-threads/query-cache). The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Qcache_not_cached`

- <strong>Description:</strong> Number of queries that are uncacheable by the [query cache](/replication/optimization-and-tuning/buffers-caches-and-threads/query-cache), or use SQL_NO_CACHE. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Qcache_queries_in_cache`

- <strong>Description:</strong> Number of queries currently cached by the [query cache](/replication/optimization-and-tuning/buffers-caches-and-threads/query-cache).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Qcache_total_blocks`

- <strong>Description:</strong> Number of blocks used by the [query cache](/replication/optimization-and-tuning/buffers-caches-and-threads/query-cache).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Queries`

- <strong>Description:</strong> Number of statements executed by the server, excluding COM_PING and COM_STATISTICS. Differs from [Questions](#questions) in that it also counts statements executed within [stored programs](/kb/en/stored-programs-and-views/).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Questions`

- <strong>Description:</strong> Number of statements executed by the server, excluding COM_PING, COM_STATISTICS, COM_STMT_PREPARE, COM_STMT_CLOSE, and COM_STMT_RESET statements. Differs from [Queries](#queries) in that it doesn't count statements executed within [stored programs](/kb/en/stored-programs-and-views/).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Rows_read`

- <strong>Description:</strong> Number of requests to read a row (excluding temporary tables).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.3](/kb/en/what-is-mariadb-53/)

---

#### `Rows_sent`

- <strong>Description:</strong>
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.3](/kb/en/what-is-mariadb-53/)

---

#### `Rows_tmp_read`

- <strong>Description:</strong> Number of requests to read a row in a temporary table.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.3](/kb/en/what-is-mariadb-53/)

---

#### `Select_full_join`

- <strong>Description:</strong> Number of joins which did not use an index. If not zero, you may need to check table indexes.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Select_full_range_join`

- <strong>Description:</strong> Number of joins which used a range search of the first table.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Select_range`

- <strong>Description:</strong> Number of joins which used a range on the first table.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Select_range_check`

- <strong>Description:</strong> Number of joins without keys that check for key usage after each row. If not zero, you may need to check table indexes.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Select_scan`

- <strong>Description:</strong> Number of joins which used a full scan of the first table.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Slow_launch_threads`

- <strong>Description:</strong> Number of threads which took longer than [slow_launch_time](/kb/en/server-system-variables/#slow_launch_time) to create. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Slow_queries`

- <strong>Description:</strong> Number of queries which took longer than [long_query_time](/kb/en/server-system-variables/#long_query_time) to run. The [slow query log](/mariadb-administration/server-monitoring-logs/slow-query-log) does not need to be active for this to be recorded.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Sort_merge_passes`

- <strong>Description:</strong> Number of merge passes performed by the sort algorithm. If too high, you may need to look at improving your query indexes, or increasing the [sort_buffer_size](/kb/en/server-system-variables/#sort_buffer_size).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Sort_priority_queue_sorts`

- <strong>Description:</strong> The number of times that sorting was done through a priority queue. (The total number of times sorting was done is a sum [Sort_range](#sort_range) and [Sort_scan](#sort_scan)). See [filesort with small LIMIT optimization](/replication/optimization-and-tuning/query-optimizations/filesort-with-small-limit-optimization).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.0.13](/kb/en/mariadb-10013-release-notes/)

---

#### `Sort_range`

- <strong>Description:</strong> Number of sorts which used a range.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Sort_rows`

- <strong>Description:</strong> Number of rows sorted.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Sort_scan`

- <strong>Description:</strong> Number of sorts which used a full table scan.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Subquery_cache_hit`

- <strong>Description:</strong> Counter for all [subquery cache](/replication/optimization-and-tuning/query-optimizations/subquery-optimizations/subquery-cache) hits. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.3](/kb/en/what-is-mariadb-53/)

---

#### `Subquery_cache_miss`

- <strong>Description:</strong> Counter for all [subquery cache](/replication/optimization-and-tuning/query-optimizations/subquery-optimizations/subquery-cache) misses. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.3](/kb/en/what-is-mariadb-53/)

---

#### `Syncs`

- <strong>Description:</strong> Number of times my_sync() has been called, or the number of times the server has had to force data to disk. Covers the [binary log](/mariadb-administration/server-monitoring-logs/binary-log), .frm creation (if these
operations are configured to sync) and some storage engines ([Archive](/columns-storage-engines-and-plugins/storage-engines/archive),
[CSV](/columns-storage-engines-and-plugins/storage-engines/csv), [Aria](/columns-storage-engines-and-plugins/storage-engines/aria)), but not [XtraDB/InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb)).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 5.1](/kb/en/what-is-mariadb-51/)

---

#### `Table_locks_immediate`

- <strong>Description:</strong> Number of table locks which were completed immediately. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Table_locks_waited`

- <strong>Description:</strong> Number of table locks which had to wait. Indicates table lock contention. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Table_open_cache_active_instances`

- <strong>Description:</strong> Number of active instances for open tables cache lookups.
- <strong>Scope:</strong>
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/)

---

#### `Table_open_cache_hits`

- <strong>Description:</strong> Number of hits for open tables cache lookups.
- <strong>Scope:</strong>
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/)

---

#### `Table_open_cache_misses`

- <strong>Description:</strong> Number of misses for open tables cache lookups.
- <strong>Scope:</strong>
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/)

---

#### `Table_open_cache_overflows`

- <strong>Description:</strong> Number of overflows for open tables cache lookups.
- <strong>Scope:</strong>
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/)

---

#### `Tc_log_max_pages_used`

- <strong>Description:</strong> Max number of pages used by the memory-mapped file-based [transaction coordinator log](/mariadb-administration/server-monitoring-logs/transaction-coordinator-log). The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Tc_log_page_size`

- <strong>Description:</strong> Page size of the memory-mapped file-based [transaction coordinator log](/mariadb-administration/server-monitoring-logs/transaction-coordinator-log).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Tc_log_page_waits`

- <strong>Description:</strong> Number of times a two-phase commit was forced to wait for a free memory-mapped file-based [transaction coordinator log](/mariadb-administration/server-monitoring-logs/transaction-coordinator-log) page. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Threads_cached`

- <strong>Description:</strong> Number of threads cached in the thread cache. This value will be zero if the [thread pool](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-pool/thread-pool-in-mariadb) is in use.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Threads_connected`

- <strong>Description:</strong> Number of clients connected to the server. See [Handling Too Many Connections](/replication/optimization-and-tuning/system-variables/handling-too-many-connections). The `Threads_connected` name is inaccurate when the [thread pool](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-pool/thread-pool-in-mariadb) is in use, since each client connection does not correspond to a dedicated thread in that case.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Threads_created`

- <strong>Description:</strong> Number of threads created to respond to client connections. If too large, look at increasing [thread_cache_size](/kb/en/server-system-variables/#thread_cache_size).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Threads_running`

- <strong>Description:</strong> Number of client connections that are actively running a command, and not just sleeping while waiting to receive the next command to execute. Some internal system threads also count towards this status variable if they would show up in the output of the [SHOW PROCESSLIST](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-processlist) statement.
<ul start="1"><li>In [MariaDB 10.3.2](/kb/en/mariadb-1032-release-notes/) and before, a global counter was updated each time a client connection dispatched a command. In these versions, the global and session status variable are always the same value.
</li><li>In [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/) and later, the global counter has been removed as a performance improvement. Instead, when the global status variable is queried, it is calculated dynamically by essentially adding up all the running client connections as they would appear in [SHOW PROCESSLIST](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-processlist) output. A client connection is only considered to be running if its thread [COMMAND](/replication/optimization-and-tuning/buffers-caches-and-threads/thread-command-values) value is not equal to `Sleep`. When the session status variable is queried, it always returns `1`.
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Update_scan`

- <strong>Description:</strong> Number of updates that required a full table scan.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> [MariaDB 10.1.17](/kb/en/mariadb-10117-release-notes/), [MariaDB 10.0.27](/kb/en/mariadb-10027-release-notes/)

---

#### `Uptime`

- <strong>Description:</strong> Number of seconds the server has been running.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Uptime_since_flush_status`

- <strong>Description:</strong> Number of seconds since the last [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---