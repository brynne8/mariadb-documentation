# Upgrading from MariaDB 5.3 to MariaDB 5.5

## What you need to know

There are no changes in table or index formats between [MariaDB 5.3](/kb/en/what-is-mariadb-53/) and [MariaDB
5.5](/kb/en/what-is-mariadb-55/), so on most servers the upgrade should be painless.

### How to upgrade

The suggested upgrade procedure is:

1 For Windows, see [Upgrading MariaDB on Windows](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-mariadb-on-windows/) instead.
2 Shutdown [MariaDB 5.3](/kb/en/what-is-mariadb-53/)
3 Take a backup (this is the perfect time to take a backup of your databases)
4 Uninstall [MariaDB 5.3](/kb/en/what-is-mariadb-53/)
5 Install [MariaDB 5.5](/kb/en/what-is-mariadb-55/) <sup class="reference" id="_ref-0">[[1](#_note-0)]</sup>
6 Run [mysql_upgrade](/sql-statements-structure/sql-statements/table-statements/mysql_upgrade/)
<ul start="1"><li>Ubuntu and Debian packages do this automatically when they are installed; Red Hat, CentOS, and Fedora packages do not
</li><li>`mysql_upgrade` does two things:
<ol start="1"><li>Upgrades the permission tables in the `mysql` database with some new fields
</li><li>Does a very quick check of all tables and marks them as compatible with [MariaDB 5.5](/kb/en/what-is-mariadb-55/)
</li></ol>
</li><li>In most cases this should be a fast operation (depending of course on the number of tables)
</li></ul>
7 Add new options to [my.cnf](/kb/en/configuring-mariadb-with-mycnf/) to enable features
<ul start="1"><li>If you change <code class="highlight fixed" style="white-space:pre-wrap">my.cnf</code> then you need to restart `mysqld`
</li></ul>

### Incompatible changes between 5.3 and 5.5

As mentioned previously, on most servers upgrading from 5.5 should be painless.
However, there are some things that have changed which could affect an upgrade:

#### XtraDB options that have changed default values

<table><tbody><tr><th>Option</th><th>Old value</th><th>New value</th></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_change_buffering">innodb_change_buffering</a></td><td>inserts</td><td>all</td></tr>
<tr><td><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_flush_neighbor_pages">innodb_flush_neighbor_pages</a></td><td>1</td><td>area</td></tr>
</tbody></table>

#### Options that have been removed or renamed

Percona, the provider of [XtraDB](/kb/en/xtradb-and-innodb/), does not provide all earlier XtraDB features in the 5.5 code base. Because of that, [MariaDB 5.5](/kb/en/what-is-mariadb-55/) can't provide them either. The following options are not supported by XtraDB 5.5. If you are using them in any of your my.cnf files, you should remove them before upgrading to 5.5.

- [innodb_adaptive_checkpoint](/kb/en/xtradbinnodb-server-system-variables/#innodb_adaptive_checkpoint); Use
  [innodb_adaptive_flushing_method](/kb/en/xtradbinnodb-server-system-variables/#innodb_adaptive_flushing_method) instead.
- [innodb_auto_lru_dump](/kb/en/xtradbinnodb-server-system-variables/#innodb_auto_lru_dump); Use  [innodb_buffer_pool_restore_at_startup](/kb/en/xtradbinnodb-server-system-variables/#innodb_buffer_pool_restore_at_startup) instead (and [innodb_buffer_pool_load_at_startup](/kb/en/xtradbinnodb-server-system-variables/#innodb_buffer_pool_load_at_startup) in [MariaDB 10.0](/kb/en/what-is-mariadb-100/)).
- [innodb_blocking_lru_restore](/kb/en/xtradbinnodb-server-system-variables/#innodb_blocking_lru_restore); Use
   [innodb_blocking_buffer_pool_restore](/kb/en/xtradbinnodb-server-system-variables/#innodb_blocking_buffer_pool_restore) instead.
- [innodb_enable_unsafe_group_commit](/kb/en/xtradbinnodb-server-system-variables/#innodb_enable_unsafe_group_commit)
- [innodb_expand_import](/kb/en/xtradbinnodb-server-system-variables/#innodb_expand_import); Use
  [innodb_import_table_from_xtrabackup](/kb/en/xtradbinnodb-server-system-variables/#innodb_import_table_from_xtrabackup) instead.
- [innodb_extra_rsegments](/kb/en/xtradbinnodb-server-system-variables/#innodb_extra_rsegments); Use [innodb_rollback_segments](/kb/en/xtradbinnodb-server-system-variables/#innodb_rollback_segments) instead.
- [innodb_extra_undoslots](/kb/en/xtradbinnodb-server-system-variables/#innodb_extra_undoslots)
- [innodb_fast_recovery](/kb/en/xtradbinnodb-server-system-variables/#innodb_fast_recovery)
- [innodb_flush_log_at_trx_commit_session](/kb/en/xtradbinnodb-server-system-variables/#innodb_flush_log_at_trx_commit_session)
- [innodb_overwrite_relay_log_info](/kb/en/xtradbinnodb-server-system-variables/#innodb_overwrite_relay_log_info)
- [innodb_pass_corrupt_table](/kb/en/xtradbinnodb-server-system-variables/#innodb_pass_corrupt_table); Use
  [innodb_corrupt_table_action](/kb/en/xtradbinnodb-server-system-variables/#innodb_corrupt_table_action) instead.
- [innodb_use_purge_thread](/kb/en/xtradbinnodb-server-system-variables/#innodb_use_purge_thread)
- [xtradb_enhancements](/kb/en/xtradbinnodb-server-system-variables/#xtradb_enhancements)

## Notes

1 [↑](#_ref-0) If using a MariaDB `apt` or `yum` [repository](https://downloads.mariadb.org/mariadb/repositories/), it is often enough to replace instances of '5.3' with '5.5' and then run an update/upgrade. For example, in Ubuntu/Debian update the MariaDB `sources.list` entry from something that looks similar to this:<pre class="fixed">deb http://ftp.osuosl.org/pub/mariadb/repo/5.3/ubuntu trusty main
</pre>To something like this:<pre class="fixed">deb http://ftp.osuosl.org/pub/mariadb/repo/5.5/ubuntu trusty main
</pre>And then run <pre class="fixed">apt-get update <span class="o">&amp;&amp;</span> apt-get upgrade
</pre>And in Red Hat, CentOS, and Fedora, change the `baseurl` line from something that looks like this:<pre class="fixed"><span class="nv">baseurl</span> <span class="o">=</span> http://yum.mariadb.org/5.3/centos6-amd64
</pre>To something that looks like this:<pre class="fixed"><span class="nv">baseurl</span> <span class="o">=</span> http://yum.mariadb.org/5.5/centos6-amd64
</pre> And then run <pre class="fixed">yum update
</pre>

## See also

- [The features in MariaDB 5.5](/kb/en/what-is-mariadb-55/)
- [Perconas guide of how to upgrade to 5.5](http://www.percona.com/doc/percona-server/5.5/upgrading_guide_51_55.html)