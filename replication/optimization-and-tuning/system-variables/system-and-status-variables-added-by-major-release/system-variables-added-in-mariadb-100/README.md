# System Variables Added in MariaDB 10.0

This is a list of [system variables](/replication/optimization-and-tuning/system-variables/server-system-variables) that were added in the [MariaDB 10.0](/kb/en/what-is-mariadb-100/) series.

The list excludes the following variables, related to storage engines and plugins included in [MariaDB 10.0](/kb/en/what-is-mariadb-100/):

- [Connect System Variables](/columns-storage-engines-and-plugins/storage-engines/connect/connect-system-variables)
- [Galera System Variables](/replication/galera-cluster/galera-cluster-system-variables)
- [Mroonga System Variables](/columns-storage-engines-and-plugins/storage-engines/mroonga/mroonga-system-variables)
- [Query Response Time Plugin Variables](/kb/en/query_response_time-plugin/)
- [Spider System Variables](/columns-storage-engines-and-plugins/storage-engines/spider/spider-server-system-variables)

<table><tbody><tr><th>Variable</th><th>Added</th></tr>
<tr><td><a href="/kb/en/aria-system-variables/#aria_pagecache_file_hash_size">aria_pagecache_file_hash_size</a></td><td><a href="/kb/en/mariadb-10013-release-notes/">MariaDB 10.0.13</a></td></tr>
<tr><td><a href="/kb/en/replication-and-binary-log-server-system-variables/#binlog_commit_wait_count">binlog_commit_wait_count</a></td><td><a href="/kb/en/mariadb-1005-release-notes/">MariaDB 10.0.5</a></td></tr>
<tr><td><a href="/kb/en/replication-and-binary-log-server-system-variables/#binlog_commit_wait_usec">binlog_commit_wait_usec</a></td><td><a href="/kb/en/mariadb-1005-release-notes/">MariaDB 10.0.5</a></td></tr>
<tr><td><a href="/kb/en/replication-and-binary-log-server-system-variables/#default_master_connection">default_master_connection</a></td><td><a href="/kb/en/mariadb-1001-release-notes/">MariaDB 10.0.1</a></td></tr>
<tr><td><a href="/kb/en/server-system-variables/#default_regex_flags">default_regex_flags</a></td><td><a href="/kb/en/mariadb-10011-release-notes/">MariaDB 10.0.11</a></td></tr>
<tr><td><a href="/kb/en/global-transaction-id/#gtid_binlog_pos">gtid_binlog_pos</a></td><td><a href="/kb/en/mariadb-1003-release-notes/">MariaDB 10.0.3</a></td></tr>
<tr><td><a href="/kb/en/global-transaction-id/#gtid_binlog_state">gtid_binlog_state</a></td><td><a href="/kb/en/mariadb-1005-release-notes/">MariaDB 10.0.5</a></td></tr>
<tr><td><a href="/kb/en/global-transaction-id/#gtid_current_pos">gtid_current_pos</a></td><td><a href="/kb/en/mariadb-1003-release-notes/">MariaDB 10.0.3</a></td></tr>
<tr><td><a href="/kb/en/global-transaction-id/#gtid_domain_id">gtid_domain_id</a></td><td><a href="/kb/en/mariadb-1002-release-notes/">MariaDB 10.0.2</a></td></tr>
<tr><td><a href="/kb/en/global-transaction-id/#gtid_ignore_duplicates">gtid_ignore_duplicates</a></td><td><a href="/kb/en/mariadb-10010-release-notes/">MariaDB 10.0.10</a></td></tr>
<tr><td><a href="/kb/en/global-transaction-id/#gtid_seq_no">gtid_seq_no</a></td><td><a href="/kb/en/mariadb-1002-release-notes/">MariaDB 10.0.2</a></td></tr>
<tr><td><a href="/kb/en/global-transaction-id/#gtid_slave_pos">gtid_slave_pos</a></td><td><a href="/kb/en/mariadb-1003-release-notes/">MariaDB 10.0.3</a></td></tr>
<tr><td><a href="/kb/en/global-transaction-id/#gtid_strict_mode">gtid_strict_mode</a></td><td><a href="/kb/en/mariadb-1003-release-notes/">MariaDB 10.0.3</a></td></tr>
<tr><td><a href="/kb/en/server-system-variables/#histogram_size">histogram_size</a></td><td><a href="/kb/en/mariadb-1002-release-notes/">MariaDB 10.0.2</a></td></tr>
<tr><td><a href="/kb/en/server-system-variables/#histogram_type">histogram_type</a></td><td><a href="/kb/en/mariadb-1002-release-notes/">MariaDB 10.0.2</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_adaptive_flushing_lwm">innodb_adaptive_flushing_lwm</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_adaptive_max_sleep_delay">innodb_adaptive_max_sleep_delay</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_api_bk_commit_interval">innodb_api_bk_commit_interval</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_api_disable_rowlock">innodb_api_disable_rowlock</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_api_enable_binlog">innodb_api_enable_binlog</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_api_enable_mdl">innodb_api_enable_mdl</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_api_trx_level">innodb_api_trx_level</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_buffer_pool_dump_at_shutdown">innodb_buffer_pool_dump_at_shutdown</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_buffer_pool_dump_now">innodb_buffer_pool_dump_now</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_buffer_pool_filename">innodb_buffer_pool_filename</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_buffer_pool_load_abort">innodb_buffer_pool_load_abort</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_buffer_pool_load_at_startup">innodb_buffer_pool_load_at_startup</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_buffer_pool_load_now">innodb_buffer_pool_load_now</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_change_buffer_max_size">innodb_change_buffer_max_size</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_checksum_algorithm">innodb_checksum_algorithm</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_cleaner_lsn_age_factor">innodb_cleaner_lsn_age_factor</a></td><td><a href="/kb/en/mariadb-1009-release-notes/">MariaDB 10.0.9</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_cmp_per_index_enabled">innodb_cmp_per_index_enabled</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_compression_failure_threshold_pct">innodb_compression_failure_threshold_pct</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_compression_level">innodb_compression_level</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_compression_pad_pct_max">innodb_compression_pad_pct_max</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_disable_sort_file_cachex">innodb_disable_sort_file_cache</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_empty_free_list_algorithm">innodb_empty_free_list_algorithm</a></td><td><a href="/kb/en/mariadb-1009-release-notes/">MariaDB 10.0.9</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_flush_neighbors">innodb_flush_neighbors</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_foreground_preflush">innodb_foreground_preflush</a></td><td><a href="/kb/en/mariadb-1009-release-notes/">MariaDB 10.0.9</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_ft_aux_table">innodb_ft_aux_table</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_ft_cache_size">innodb_ft_cache_size</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_ft_enable_diag_print">innodb_ft_enable_diag_print</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_ft_enable_stopword">innodb_ft_enable_stopword</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_ft_max_token_size">innodb_ft_max_token_size</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_ft_min_token_size">innodb_ft_min_token_size</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_ft_num_word_optimize">innodb_ft_num_word_optimize</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_ft_result_cache_limit">innodb_ft_result_cache_limit</a></td><td><a href="/kb/en/mariadb-1009-release-notes/">MariaDB 10.0.9</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_ft_server_stopword_table">innodb_ft_server_stopword_table</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_ft_sort_pll_degree">innodb_ft_sort_pll_degree</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_ft_total_cache_size">innodb_ft_total_cache_size</a></td><td><a href="/kb/en/mariadb-1009-release-notes/">MariaDB 10.0.9</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_ft_user_stopword_table">innodb_ft_user_stopword_table</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_io_capacity_max">innodb_io_capacity_max</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_log_arch_dir">innodb_log_arch_dir</a></td><td><a href="/kb/en/mariadb-1009-release-notes/">MariaDB 10.0.9</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_log_arch_expire_sec">innodb_log_arch_expire_sec</a></td><td><a href="/kb/en/mariadb-1009-release-notes/">MariaDB 10.0.9</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_log_archive">innodb_log_archive</a></td><td><a href="/kb/en/mariadb-1009-release-notes/">MariaDB 10.0.9</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_log_compressed_pages">innodb_log_compressed_pages</a></td><td><a href="/kb/en/mariadb-1009-release-notes/">MariaDB 10.0.9</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_max_dirty_pages_pct_lwm">innodb_max_dirty_pages_pct_lwm</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_max_purge_lag_delay">innodb_max_purge_lag_delay</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_monitor_disable">innodb_monitor_disable</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_monitor_enable">innodb_monitor_enable</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_monitor_reset">innodb_monitor_reset</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_monitor_reset_all">innodb_monitor_reset_all</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_online_alter_log_max_size">innodb_online_alter_log_max_size</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_optimize_fulltext_only">innodb_optimize_fulltext_only</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_random_read_ahead">innodb_random_read_ahead</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_read_only">innodb_read_only</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_recovery_stats">innodb_recovery_stats</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_sched_priority_cleaner">innodb_sched_priority_cleaner</a></td><td><a href="/kb/en/mariadb-1009-release-notes/">MariaDB 10.0.9</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_simulate_comp_failures">innodb_simulate_comp_failures</a></td><td><a href="/kb/en/mariadb-10014-release-notes/">MariaDB 10.0.14</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_sort_buffer_size">innodb_sort_buffer_size</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_stats_auto_recalc">innodb_stats_auto_recalc</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_stats_modified_counter">innodb_stats_modified_counter</a></td><td><a href="/kb/en/mariadb-10015-release-notes/">MariaDB 10.0.15</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_stats_persistent">innodb_stats_persistent</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_stats_persistent_sample_pages">innodb_stats_persistent_sample_pages</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_stats_traditional">innodb_stats_traditional</a></td><td><a href="/kb/en/mariadb-10016-release-notes/">MariaDB 10.0.16</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_stats_transient_sample_pages">innodb_stats_transient_sample_pages</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_status_output">innodb_status_output</a></td><td><a href="/kb/en/mariadb-10014-release-notes/">MariaDB 10.0.14</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_status_output_locks">innodb_status_output_locks</a></td><td><a href="/kb/en/mariadb-10014-release-notes/">MariaDB 10.0.14</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_sync_array_size">innodb_sync_array_size</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_undo_directory">innodb_undo_directory</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_undo_logs">innodb_undo_logs</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_undo_tablespaces">innodb_undo_tablespaces</a></td><td><a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td><a href="/kb/en/myisam-system-variables/#key_cache_file_hash_size">key_cache_file_hash_size</a></td><td><a href="/kb/en/mariadb-10013-release-notes/">MariaDB 10.0.13</a></td></tr>
<tr><td><a href="/kb/en/global-transaction-id/#last_gtid">last_gtid</a></td><td><a href="/kb/en/mariadb-1009-release-notes/">MariaDB 10.0.9</a></td></tr>
<tr><td><a href="/kb/en/server-system-variables/#oqgraph_allow_create_integer_latch">oqgraph_allow_create_integer_latch</a></td><td><a href="/kb/en/mariadb-1007-release-notes/">MariaDB 10.0.7</a></td></tr>
<tr><td><a href="/kb/en/performance-schema-system-variables/#performance_schema_accounts_size">performance_schema_accounts_size</a></td><td><a href="/kb/en/mariadb-1000-release-notes/">MariaDB 10.0.0</a></td></tr>
<tr><td><a href="/kb/en/performance-schema-system-variables/#performance_schema_digests_size">performance_schema_digests_size</a></td><td><a href="/kb/en/mariadb-1000-release-notes/">MariaDB 10.0.0</a></td></tr>
<tr><td><a href="/kb/en/performance-schema-system-variables/#performance_schema_events_stages_history_long_size">performance_schema_events_stages_history_long_size</a></td><td><a href="/kb/en/mariadb-1000-release-notes/">MariaDB 10.0.0</a></td></tr>
<tr><td><a href="/kb/en/performance-schema-system-variables/#performance_schema_events_stages_history_size">performance_schema_events_stages_history_size</a></td><td><a href="/kb/en/mariadb-1000-release-notes/">MariaDB 10.0.0</a></td></tr>
<tr><td><a href="/kb/en/performance-schema-system-variables/#performance_schema_events_statements_history_long_size">performance_schema_events_statements_history_long_size</a></td><td><a href="/kb/en/mariadb-1000-release-notes/">MariaDB 10.0.0</a></td></tr>
<tr><td><a href="/kb/en/performance-schema-system-variables/#performance_schema_events_statements_history_size">performance_schema_events_statements_history_size</a></td><td><a href="/kb/en/mariadb-1000-release-notes/">MariaDB 10.0.0</a></td></tr>
<tr><td><a href="/kb/en/performance-schema-system-variables/#performance_schema_hosts_size">performance_schema_hosts_size</a></td><td><a href="/kb/en/mariadb-1000-release-notes/">MariaDB 10.0.0</a></td></tr>
<tr><td><a href="/kb/en/performance-schema-system-variables/#performance_schema_max_digest_length">performance_schema_max_digest_length</a></td><td><a href="/kb/en/mariadb-10021-release-notes/">MariaDB 10.0.21</a></td></tr>
<tr><td><a href="/kb/en/performance-schema-system-variables/#performance_schema_max_socket_classes">performance_schema_max_socket_classes</a></td><td><a href="/kb/en/mariadb-1000-release-notes/">MariaDB 10.0.0</a></td></tr>
<tr><td><a href="/kb/en/performance-schema-system-variables/#performance_schema_max_socket_instances">performance_schema_max_socket_instances</a></td><td><a href="/kb/en/mariadb-1000-release-notes/">MariaDB 10.0.0</a></td></tr>
<tr><td><a href="/kb/en/performance-schema-system-variables/#performance_schema_max_stage_classes">performance_schema_max_stage_classes</a></td><td><a href="/kb/en/mariadb-1000-release-notes/">MariaDB 10.0.0</a></td></tr>
<tr><td><a href="/kb/en/performance-schema-system-variables/#performance_schema_max_statement_classes">performance_schema_max_statement_classes</a></td><td><a href="/kb/en/mariadb-1000-release-notes/">MariaDB 10.0.0</a></td></tr>
<tr><td><a href="/kb/en/performance-schema-system-variables/#performance_schema_session_connect_attrs_size">performance_schema_session_connect_attrs_size</a></td><td><a href="/kb/en/mariadb-1004-release-notes/">MariaDB 10.0.4</a></td></tr>
<tr><td><a href="/kb/en/performance-schema-system-variables/#performance_schema_setup_actors_size">performance_schema_setup_actors_size</a></td><td><a href="/kb/en/mariadb-1000-release-notes/">MariaDB 10.0.0</a></td></tr>
<tr><td><a href="/kb/en/performance-schema-system-variables/#performance_schema_setup_objects_size">performance_schema_setup_objects_size</a></td><td><a href="/kb/en/mariadb-1000-release-notes/">MariaDB 10.0.0</a></td></tr>
<tr><td><a href="/kb/en/performance-schema-system-variables/#performance_schema_users_size">performance_schema_users_size</a></td><td><a href="/kb/en/mariadb-1000-release-notes/">MariaDB 10.0.0</a></td></tr>
<tr><td><a href="/kb/en/replication-and-binary-log-server-system-variables/#slave_ddl_exec_mode">slave_ddl_exec_mode</a></td><td><a href="/kb/en/mariadb-1008-release-notes/">MariaDB 10.0.8</a></td></tr>
<tr><td><a href="/kb/en/replication-and-binary-log-server-system-variables/#slave_domain_parallel_threads">slave_domain_parallel_threads</a></td><td><a href="/kb/en/mariadb-1009-release-notes/">MariaDB 10.0.9</a></td></tr>
<tr><td><a href="/kb/en/replication-and-binary-log-server-system-variables/#slave_parallel_max_queued">slave_parallel_max_queued</a></td><td><a href="/kb/en/mariadb-1005-release-notes/">MariaDB 10.0.5</a></td></tr>
<tr><td><a href="/kb/en/replication-and-binary-log-server-system-variables/#slave_parallel_threads">slave_parallel_threads</a></td><td><a href="/kb/en/mariadb-1005-release-notes/">MariaDB 10.0.5</a></td></tr>
<tr><td><a href="/kb/en/ssl-system-variables/#ssl_crl">ssl_crl</a></td><td><a href="/kb/en/mariadb-1000-release-notes/">MariaDB 10.0.0</a></td></tr>
<tr><td><a href="/kb/en/ssl-system-variables/#ssl_crlpath">ssl_crlpath</a></td><td><a href="/kb/en/mariadb-1000-release-notes/">MariaDB 10.0.0</a></td></tr>
<tr><td><a href="/kb/en/server-system-variables/#use_stat_tables">use_stat_tables</a></td><td><a href="/kb/en/mariadb-1001-release-notes/">MariaDB 10.0.1</a></td></tr>
<tr><td><a href="/kb/en/server-system-variables/#version_malloc_library">version_malloc_library</a></td><td><a href="/kb/en/mariadb-1001-release-notes/">MariaDB 10.0.1</a></td></tr>
</tbody></table>

## See Also

- [Status Variables Added in MariaDB 10.0](/replication/optimization-and-tuning/system-variables/system-and-status-variables-added-by-major-release/status-variables-added-in-mariadb-100)
- [System Variables Added in MariaDB 10.1](/replication/optimization-and-tuning/system-variables/system-and-status-variables-added-by-major-release/system-variables-added-in-mariadb-101)
- [Upgrading from MariaDB 5.5 to MariaDB 10.0](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-from-mariadb-55-to-mariadb-100)