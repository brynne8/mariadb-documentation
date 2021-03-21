# Status Variables Added in MariaDB 10.1

This is a list of [status variables](/replication/optimization-and-tuning/system-variables/server-status-variables) that were added in the [MariaDB 10.1](/kb/en/what-is-mariadb-101/) series.

The list excludes status related to the following storage engines included in [MariaDB 10.1](/kb/en/what-is-mariadb-101/):

- [Galera Status Variables](/replication/galera-cluster/galera-cluster-status-variables)

<table><tbody><tr><th>Variable</th><th>Added</th></tr>
<tr><td><a href="/kb/en/server-status-variables/#acl_column_grants">Acl_column_grants</a></td><td><a href="/kb/en/mariadb-1014-release-notes/">MariaDB 10.1.4</a></td></tr>
<tr><td><a href="/kb/en/server-status-variables/#acl_database_grants">Acl_database_grants</a></td><td><a href="/kb/en/mariadb-1014-release-notes/">MariaDB 10.1.4</a></td></tr>
<tr><td><a href="/kb/en/server-status-variables/#acl_function_grants">Acl_function_grants</a></td><td><a href="/kb/en/mariadb-1014-release-notes/">MariaDB 10.1.4</a></td></tr>
<tr><td><a href="/kb/en/server-status-variables/#acl_procedure_grants">Acl_procedure_grants</a></td><td><a href="/kb/en/mariadb-1014-release-notes/">MariaDB 10.1.4</a></td></tr>
<tr><td><a href="/kb/en/server-status-variables/#acl_proxy_users">Acl_proxy_users</a></td><td><a href="/kb/en/mariadb-1014-release-notes/">MariaDB 10.1.4</a></td></tr>
<tr><td><a href="/kb/en/server-status-variables/#acl_role_grants">Acl_role_grants</a></td><td><a href="/kb/en/mariadb-1014-release-notes/">MariaDB 10.1.4</a></td></tr>
<tr><td><a href="/kb/en/server-status-variables/#acl_roles">Acl_roles</a></td><td><a href="/kb/en/mariadb-1014-release-notes/">MariaDB 10.1.4</a></td></tr>
<tr><td><a href="/kb/en/server-status-variables/#acl_table_grants">Acl_table_grants</a></td><td><a href="/kb/en/mariadb-1014-release-notes/">MariaDB 10.1.4</a></td></tr>
<tr><td><a href="/kb/en/server-status-variables/#acl_users">Acl_users</a></td><td><a href="/kb/en/mariadb-1014-release-notes/">MariaDB 10.1.4</a></td></tr>
<tr><td><a href="/kb/en/replication-and-binary-log-status-variables/#master_gtid_wait_count">Master_gtid_wait_count</a></td><td><a href="/kb/en/mariadb-1014-release-notes/">MariaDB 10.1.4</a></td></tr>
<tr><td><a href="/kb/en/server-status-variables/#com_compound_sql">Com_compound_sql</a></td><td><a href="/kb/en/mariadb-1011-release-notes/">MariaDB 10.1.1</a></td></tr>
<tr><td><a href="/kb/en/server-status-variables/#com_create_temporary_table">Com_create_temporary_table</a></td><td><a href="/kb/en/mariadb-1016-release-notes/">MariaDB 10.1.6</a></td></tr>
<tr><td><a href="/kb/en/server-status-variables/#com_create_drop_table">Com_create_drop_table</a></td><td><a href="/kb/en/mariadb-1016-release-notes/">MariaDB 10.1.6</a></td></tr>
<tr><td><a href="/kb/en/server-status-variables/#com_show_generic">Com_show_generic</a></td><td><a href="/kb/en/mariadb-1011-release-notes/">MariaDB 10.1.1</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_defragment_compression_failures">Innodb_defragment_compression_failures</a></td><td><a href="/kb/en/mariadb-1011-release-notes/">MariaDB 10.1.1</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_defragment_count">Innodb_defragment_count</a></td><td><a href="/kb/en/mariadb-1011-release-notes/">MariaDB 10.1.1</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_defragment_failures">Innodb_defragment_failures</a></td><td><a href="/kb/en/mariadb-1011-release-notes/">MariaDB 10.1.1</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_encryption_rotation_estimated_iops">Innodb_encryption_rotation_estimated_iops</a></td><td><a href="/kb/en/mariadb-1013-release-notes/">MariaDB 10.1.3</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_encryption_rotation_pages_flushed">Innodb_encryption_rotation_pages_flushed</a></td><td><a href="/kb/en/mariadb-1013-release-notes/">MariaDB 10.1.3</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_encryption_rotation_pages_modified">Innodb_encryption_rotation_pages_modified</a></td><td><a href="/kb/en/mariadb-1013-release-notes/">MariaDB 10.1.3</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_encryption_rotation_pages_read_from_cache">Innodb_encryption_rotation_pages_read_from_cache</a></td><td><a href="/kb/en/mariadb-1013-release-notes/">MariaDB 10.1.3</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_encryption_rotation_pages_read_from_disk">Innodb_encryption_rotation_pages_read_from_disk</a></td><td><a href="/kb/en/mariadb-1013-release-notes/">MariaDB 10.1.3</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_have_bzip2">Innodb_have_bzip2</a></td><td><a href="/kb/en/mariadb-1010-release-notes/">MariaDB 10.1.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_have_lz4">Innodb_have_lz4</a></td><td><a href="/kb/en/mariadb-1010-release-notes/">MariaDB 10.1.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_have_lzma">Innodb_have_lzma</a></td><td><a href="/kb/en/mariadb-1010-release-notes/">MariaDB 10.1.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_have_lzo">Innodb_have_lzo</a></td><td><a href="/kb/en/mariadb-1010-release-notes/">MariaDB 10.1.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_have_snappy">Innodb_have_snappy</a></td><td><a href="/kb/en/mariadb-1013-release-notes/">MariaDB 10.1.3</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_num_index_pages_written">Innodb_num_index_pages_written</a></td><td><a href="/kb/en/mariadb-1010-release-notes/">MariaDB 10.1.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_num_non_index_pages_written">Innodb_num_non_index_pages_written</a></td><td><a href="/kb/en/mariadb-1010-release-notes/">MariaDB 10.1.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_onlineddl_pct_progress">Innodb_onlineddl_pct_progress</a></td><td><a href="/kb/en/mariadb-1011-release-notes/">MariaDB 10.1.1</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_onlineddl_rowlog_pct_used">Innodb_onlineddl_rowlog_pct_used</a></td><td><a href="/kb/en/mariadb-1011-release-notes/">MariaDB 10.1.1</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_onlineddl_rowlog_rows">Innodb_onlineddl_rowlog_rows</a></td><td><a href="/kb/en/mariadb-1011-release-notes/">MariaDB 10.1.1</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_num_page_compressed_trim_op">Innodb_num_page_compressed_trim_op</a></td><td><a href="/kb/en/mariadb-1010-release-notes/">MariaDB 10.1.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_num_page_compressed_trim_op_saved">Innodb_num_page_compressed_trim_op_saved</a></td><td><a href="/kb/en/mariadb-1010-release-notes/">MariaDB 10.1.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_num_pages_page_compressed">Innodb_num_pages_page_compressed</a></td><td><a href="/kb/en/mariadb-1010-release-notes/">MariaDB 10.1.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_num_pages_page_compression_error">Innodb_num_pages_page_compression_error</a></td><td><a href="/kb/en/what-is-mariadb-101/">MariaDB 10.1</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_num_pages_page_decompressed">Innodb_num_pages_page_decompressed</a></td><td><a href="/kb/en/mariadb-1010-release-notes/">MariaDB 10.1.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_num_pages_page_decrypted">Innodb_num_pages_page_decrypted</a></td><td><a href="/kb/en/mariadb-1013-release-notes/">MariaDB 10.1.3</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_num_pages_page_encrypted">Innodb_num_pages_page_encrypted</a></td><td><a href="/kb/en/mariadb-1013-release-notes/">MariaDB 10.1.3</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_page_compression_saved">Innodb_page_compression_saved</a></td><td><a href="/kb/en/mariadb-1010-release-notes/">MariaDB 10.1.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_page_compression_trim_sect512">Innodb_page_compression_trim_sect512</a></td><td><a href="/kb/en/mariadb-1010-release-notes/">MariaDB 10.1.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_page_compression_trim_sect1024">Innodb_page_compression_trim_sect1024</a></td><td><a href="/kb/en/mariadb-1012-release-notes/">MariaDB 10.1.2</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_page_compression_trim_sect2048">Innodb_page_compression_trim_sect2048</a></td><td><a href="/kb/en/mariadb-1012-release-notes/">MariaDB 10.1.2</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_page_compression_trim_sect4096">Innodb_page_compression_trim_sect4096</a></td><td><a href="/kb/en/mariadb-1010-release-notes/">MariaDB 10.1.0</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_page_compression_trim_sect8192">Innodb_page_compression_trim_sect8192</a></td><td><a href="/kb/en/mariadb-1012-release-notes/">MariaDB 10.1.2</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_page_compression_trim_sect16384">Innodb_page_compression_trim_sect16384</a></td><td><a href="/kb/en/mariadb-1012-release-notes/">MariaDB 10.1.2</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_page_compression_trim_sect32768">Innodb_page_compression_trim_sect32768</a></td><td><a href="/kb/en/mariadb-1012-release-notes/">MariaDB 10.1.2</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_secondary_index_triggered_cluster_reads">Innodb_secondary_index_triggered_cluster_reads</a></td><td><a href="/kb/en/mariadb-1012-release-notes/">MariaDB 10.1.2</a></td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-status-variables/#innodb_secondary_index_triggered_cluster_reads_avoided">Innodb_secondary_index_triggered_cluster_reads_avoided</a></td><td><a href="/kb/en/mariadb-1012-release-notes/">MariaDB 10.1.2</a></td></tr>
<tr><td><a href="/kb/en/replication-and-binary-log-status-variables/#master_gtid_wait_count">Master_gtid_wait_count</a></td><td><a href="/kb/en/mariadb-1014-release-notes/">MariaDB 10.1.4</a></td></tr>
<tr><td><a href="/kb/en/replication-and-binary-log-status-variables/#master_gtid_wait_time">Master_gtid_wait_time</a></td><td><a href="/kb/en/mariadb-1014-release-notes/">MariaDB 10.1.4</a></td></tr>
<tr><td><a href="/kb/en/replication-and-binary-log-status-variables/#master_gtid_wait_timeouts">Master_gtid_wait_timeouts</a></td><td><a href="/kb/en/mariadb-1014-release-notes/">MariaDB 10.1.4</a></td></tr>
<tr><td><a href="/kb/en/server-status-variables/#max_statement_time_exceeded">Max_statement_time_exceeded</a></td><td><a href="/kb/en/mariadb-1011-release-notes/">MariaDB 10.1.1</a></td></tr>
</tbody></table>

## See Also

- [System variables added in MariaDB 10.1](/replication/optimization-and-tuning/system-variables/system-and-status-variables-added-by-major-release/system-variables-added-in-mariadb-101)
- [Status variables added in MariaDB 10.2](/replication/optimization-and-tuning/system-variables/system-and-status-variables-added-by-major-release/status-variables-added-in-mariadb-102)
- [Status variables added in MariaDB 10.0](/replication/optimization-and-tuning/system-variables/system-and-status-variables-added-by-major-release/status-variables-added-in-mariadb-100)