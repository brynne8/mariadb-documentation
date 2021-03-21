# Status Variables Added in MariaDB 10.0

This is a list of [status variables](/replication/optimization-and-tuning/system-variables/server-status-variables) that were added in the [MariaDB 10.0](/kb/en/what-is-mariadb-100/) series.

The list excludes status related to the following storage engines included in [MariaDB 10.0](/kb/en/what-is-mariadb-100/):

- [Galera Status Variables](/replication/galera-cluster/galera-cluster-status-variables)
- [Mroonga Status Variables](/columns-storage-engines-and-plugins/storage-engines/mroonga/mroonga-status-variables)
- [Spider Status Variables](/kb/en/spider-server-status-variables/)

<table><tbody><tr><th>Variable</th><th>Added</th></tr>
<tr><td><a href="/kb/en/replication-and-binary-log-status-variables/#binlog_group_commit_trigger_count">Binlog_group_commit_trigger_count</a></td><td><a href="/kb/en/mariadb-10018-release-notes/">MariaDB 10.0.18</a></td></tr>
<tr><td><a href="/kb/en/replication-and-binary-log-status-variables/#binlog_group_commit_trigger_timeout">Binlog_group_commit_trigger_timeout</a></td><td><a href="/kb/en/mariadb-10018-release-notes/">MariaDB 10.0.18</a></td></tr>
<tr><td><a href="/kb/en/replication-and-binary-log-status-variables/#binlog_group_commit_trigger_lock_wait">Binlog_group_commit_trigger_lock_wait</a></td><td><a href="/kb/en/mariadb-10018-release-notes/">MariaDB 10.0.18</a></td></tr>
<tr><td><a href="/kb/en/server-status-variables/#com_create_role">Com_create_role</a></td><td><a href="/kb/en/mariadb-1005-release-notes/">MariaDB 10.0.5</a></td></tr>
<tr><td><a href="/kb/en/server-status-variables/#com_drop_role">Com_drop_role</a></td><td><a href="/kb/en/mariadb-1005-release-notes/">MariaDB 10.0.5</a></td></tr>
<tr><td><a href="/kb/en/server-status-variables/#com_get_diagnostics">Com_get_diagnostics</a></td><td><a href="/kb/en/mariadb-1004-release-notes/">MariaDB 10.0.4</a></td></tr>
<tr><td><a href="/kb/en/server-status-variables/#com_grant_role">Com_grant_role</a></td><td><a href="/kb/en/mariadb-1005-release-notes/">MariaDB 10.0.5</a></td></tr>
<tr><td><a href="/kb/en/server-status-variables/#com_revoke_grant">Com_revoke_grant</a></td><td><a href="/kb/en/mariadb-1005-release-notes/">MariaDB 10.0.5</a></td></tr>
<tr><td><a href="/kb/en/server-status-variables/#com_show_explain">Com_show_explain</a></td><td><a href="/kb/en/mariadb-1000-release-notes/">MariaDB 10.0.0</a></td></tr>
<tr><td><a href="/kb/en/replication-and-binary-log-status-variables/#com_start_all_slaves">Com_start_all_slaves</a></td><td><a href="/kb/en/mariadb-1000-release-notes/">MariaDB 10.0.0</a></td></tr>
<tr><td><a href="/kb/en/replication-and-binary-log-status-variables/#com_start_slave">Com_start_slave</a></td><td><a href="/kb/en/mariadb-1000-release-notes/">MariaDB 10.0.0</a></td></tr>
<tr><td><a href="/kb/en/replication-and-binary-log-status-variables/#com_stop_all_slaves">Com_stop_all_slaves</a></td><td><a href="/kb/en/mariadb-1000-release-notes/">MariaDB 10.0.0</a></td></tr>
<tr><td><a href="/kb/en/replication-and-binary-log-status-variables/#com_stop_slave">Com_stop_slave</a></td><td><a href="/kb/en/mariadb-1000-release-notes/">MariaDB 10.0.0</a></td></tr>
<tr><td><a href="/kb/en/server-status-variables/#connection_errors_accept">Connection_errors_accept</a></td><td><a href="/kb/en/mariadb-1004-release-notes/">MariaDB 10.0.4</a></td></tr>
<tr><td><a href="/kb/en/server-status-variables/#connection_errors_internal">Connection_errors_internal</a></td><td><a href="/kb/en/mariadb-1004-release-notes/">MariaDB 10.0.4</a></td></tr>
<tr><td><a href="/kb/en/server-status-variables/#connection_errors_max_connections">Connection_errors_max_connections</a></td><td><a href="/kb/en/mariadb-1004-release-notes/">MariaDB 10.0.4</a></td></tr>
<tr><td><a href="/kb/en/server-status-variables/#connection_errors_peer_address">Connection_errors_peer_address</a></td><td><a href="/kb/en/mariadb-1004-release-notes/">MariaDB 10.0.4</a></td></tr>
<tr><td><a href="/kb/en/server-status-variables/#connection_errors_select">Connection_errors_select</a></td><td><a href="/kb/en/mariadb-1004-release-notes/">MariaDB 10.0.4</a></td></tr>
<tr><td><a href="/kb/en/server-status-variables/#connection_errors_tcpwrap">Connection_errors_tcpwrap</a></td><td><a href="/kb/en/mariadb-1004-release-notes/">MariaDB 10.0.4</a></td></tr>
<tr><td><a href="/kb/en/server-status-variables/#delete_scan">Delete_scan</a></td><td><a href="/kb/en/mariadb-10027-release-notes/">MariaDB 10.0.27</a></td></tr>
<tr><td><a href="/kb/en/server-status-variables/#feature_delay_key_write">Feature_delay_key_write</a></td><td><a href="/kb/en/mariadb-10013-release-notes/">MariaDB 10.0.13</a></td></tr>
<tr><td><a href="/kb/en/server-status-variables/#handler_external_lock">Handler_external_lock</a></td><td><a href="/kb/en/mariadb-1000-release-notes/">MariaDB 10.0.0</a></td></tr>
<tr><td><a href="/kb/en/server-status-variables/#handler_read_retry">Handler_read_retry</a></td><td><a href="/kb/en/mariadb-10027-release-notes/">MariaDB 10.0.27</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_buffer_pool_dump_status">Innodb_buffer_pool_dump_status</a></td><td><a href="/kb/en/mariadb-1000-release-notes/">MariaDB 10.0.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_buffer_pool_load_status">Innodb_buffer_pool_load_status</a></td><td><a href="/kb/en/mariadb-1000-release-notes/">MariaDB 10.0.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_master_thread_active_loops">Innodb_master_thread_active_loops</a></td><td><a href="/kb/en/mariadb-1009-release-notes/">MariaDB 10.0.9</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_master_thread_idle_loops">Innodb_master_thread_idle_loops</a></td><td><a href="/kb/en/mariadb-1009-release-notes/">MariaDB 10.0.9</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_num_open_files">Innodb_num_open_files</a></td><td><a href="/kb/en/mariadb-1000-release-notes/">MariaDB 10.0.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_system_rows_deleted">Innodb_system_rows_deleted</a></td><td><a href="/kb/en/mariadb-10015-release-notes/">MariaDB 10.0.15</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_system_rows_inserted">Innodb_system_rows_inserted</a></td><td><a href="/kb/en/mariadb-10015-release-notes/">MariaDB 10.0.15</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_system_rows_read">Innodb_system_rows_read</a></td><td><a href="/kb/en/mariadb-10015-release-notes/">MariaDB 10.0.15</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_system_rows_updated">Innodb_system_rows_updated</a></td><td><a href="/kb/en/mariadb-10015-release-notes/">MariaDB 10.0.15</a></td></tr>
<tr><td><a href="/kb/en/server-status-variables/#memory_used">Memory_used</a></td><td><a href="/kb/en/mariadb-1001-release-notes/">MariaDB 10.0.1</a></td></tr>
<tr><td><a href="/kb/en/server-status-variables/#oqgraph_boost_version">Oqgraph_boost_version</a></td><td><a href="/kb/en/mariadb-1007-release-notes/">MariaDB 10.0.7</a></td></tr>
<tr><td><a href="/kb/en/server-status-variables/#performance_schema_accounts_lost">Performance_schema_accounts_lost</a></td><td><a href="/kb/en/mariadb-1000-release-notes/">MariaDB 10.0.0</a></td></tr>
<tr><td><a href="/kb/en/server-status-variables/#performance_schema_hosts_lost">Performance_schema_hosts_lost</a></td><td><a href="/kb/en/mariadb-1000-release-notes/">MariaDB 10.0.0</a></td></tr>
<tr><td><a href="/kb/en/server-status-variables/#performance_schema_socket_classes_lost">Performance_schema_socket_classes_lost</a></td><td><a href="/kb/en/mariadb-1000-release-notes/">MariaDB 10.0.0</a></td></tr>
<tr><td><a href="/kb/en/server-status-variables/#performance_schema_socket_instances_lost">Performance_schema_socket_instances_lost</a></td><td><a href="/kb/en/mariadb-1000-release-notes/">MariaDB 10.0.0</a></td></tr>
<tr><td><a href="/kb/en/server-status-variables/#performance_schema_stage_classes_lost">Performance_schema_stage_classes_lost</a></td><td><a href="/kb/en/mariadb-1000-release-notes/">MariaDB 10.0.0</a></td></tr>
<tr><td><a href="/kb/en/server-status-variables/#performance_schema_statement_classes_lost">Performance_schema_statement_classes_lost</a></td><td><a href="/kb/en/mariadb-1000-release-notes/">MariaDB 10.0.0</a></td></tr>
<tr><td><a href="/kb/en/replication-and-binary-log-status-variables/#slave_skipped_errors">Slave_skipped_errors</a></td><td><a href="/kb/en/mariadb-10018-release-notes/">MariaDB 10.0.18</a></td></tr>
<tr><td><a href="/kb/en/server-status-variables/#sort_priority_queue_sorts">Sort_priority_queue_sorts</a></td><td><a href="/kb/en/mariadb-10013-release-notes/">MariaDB 10.0.13</a></td></tr>
<tr><td><a href="/kb/en/server-status-variables/#update_scan">Update_scan</a></td><td><a href="/kb/en/mariadb-10027-release-notes/">MariaDB 10.0.27</a></td></tr>
</tbody></table>

## See also

- [System variables added in MariaDB 10.0](/replication/optimization-and-tuning/system-variables/system-and-status-variables-added-by-major-release/system-variables-added-in-mariadb-100)
- [Status variables added in MariaDB 10.1](/replication/optimization-and-tuning/system-variables/system-and-status-variables-added-by-major-release/status-variables-added-in-mariadb-101)