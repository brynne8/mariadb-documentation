# MariaDB Galera Cluster 5.5.34 Release Notes

The most recent [MariaDB Galera Cluster 5.5](/kb/en/galera/) release is:<br>
<span class="cstm-style lead"><strong>[MariaDB Galera Cluster 5.5.63](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5563-release-notes/)</strong> [Download<span>&nbsp;</span>Now](https://downloads.mariadb.org/mariadb-galera/5.5)</span>

[Download](http://downloads.mariadb.org/mariadb-galera/5.5.34) |
<strong>Release Notes</strong> |
[Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-changelogs/mariadb-galera-cluster-5534-changelog/) |
[Overview of MariaDB Galera Cluster](/replication/galera-cluster/what-is-mariadb-galera-cluster/)

<strong>Release date:</strong> 18 Dec 2013

MariaDB Galera Cluster 5.5.34 is a <strong><em>[Stable](/kb/en/release-criteria/)</em></strong> (GA) release.
It is a merge of [MariaDB 5.5.34](/kb/en/what-is-mariadb-55/) and
[Galera Cluster](http://codership.com/content/using-galera-cluster) with
additional bug fixes.

Various articles about MariaDB Galera Cluster, including
[known limitations](/replication/galera-cluster/mariadb-galera-cluster-known-limitations/) and
[how to get started](/replication/galera-cluster/getting-started-with-mariadb-galera-cluster/) are
available in the <strong>[Galera](/kb/en/galera/)</strong> section of the Knowledgebase.

For a list of changes made in MariaDB Galera Cluster 5.5.34, with links to detailed
information on each push, see the
[MariaDB Galera Cluster 5.5.34 Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-changelogs/mariadb-galera-cluster-5534-changelog/).

## Includes [MariaDB 5.5.34](/kb/en/mariadb-5534-release-notes/) and Galera Cluster

MariaDB Galera Cluster 5.5.34 includes [MariaDB 5.5.34](/kb/en/mariadb-5534-release-notes/) with Codership additions (`lp:codership-mysql/5.5-23` till revision `3942`) and
[Galera Cluster 23.2.7](http://codership.com/content/using-galera-cluster). See the
[MariaDB 5.5.34 Release Notes](/kb/en/mariadb-5534-release-notes/) and
[Changelog](/kb/en/mariadb-5534-changelog/) for more information on the changes in
MariaDB.

## Bug Fixes

This release contains fixes for some crashing bugs, memory leaks, compiler
errors/warnings and improvements in existing scripts.

- [MDEV-5408](https://jira.mariadb.org/browse/MDEV-5408): Crash in mariadb-wsrep during plugin load at startup
- [MDEV-5386](https://jira.mariadb.org/browse/MDEV-5386): Server crashes in thd_get_ha_data on maria-5.5-galera tree while
  running 'check testcase before test
- [MDEV-443](https://jira.mariadb.org/browse/MDEV-443): Galera: Server crashes on flushing tables for SST if started with
  character_set_server utf16 or utf32 or ucs2, and with wsrep_sst_method=rsync
- [MDEV-4227](https://jira.mariadb.org/browse/MDEV-4227): Galera server should stop crashing on setting binlog_format
  STATEMENT
- [MDEV-4222](https://jira.mariadb.org/browse/MDEV-4222): Assertion `( ((global_system_variables.wsrep_on) &amp;&amp; (thd &amp;&amp;
  thd-&gt;variables.wsrep_on))  &amp;&amp; wsrep_emulate_bin_log) || mysql_bin_log
  .is_open()' fails on SAVEPOINT with disabled wsrep_provider
- [MDEV-4235](https://jira.mariadb.org/browse/MDEV-4235): Galera: Assertion `0' fails in tdc_remove_table on creating a
  trigger
- [MDEV-4223](https://jira.mariadb.org/browse/MDEV-4223): Galera: InnoDB assertion failure !mutex_own(mutex) in file
  sync0sync.ic line 207
- [MDEV-4233](https://jira.mariadb.org/browse/MDEV-4233): Galera: assertion: (lock-&gt;trx)-&gt;wait_lock == lock fails in file
  lock0lock.c line 796

A list of all the bugs fixed can be found in the [changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-changelogs/mariadb-galera-cluster-5534-changelog/).

Thanks, and enjoy MariaDB Galera Cluster!

---

Be notified of new MariaDB Server releases automatically by [subscribing](https://lists.askmonty.org/cgi-bin/mailman/listinfo/announce) to the MariaDB Foundation community announce 'at' mariadb.org announcement list (this is a low traffic, announce-only list). MariaDB Corporation customers will be notified for all new releases, security issues and critical bug fixes for all MariaDB Corporation products thanks to the Notification Services.
<br><br>
MariaDB may already be included in your favorite OS distribution. More
information can be found on the
[Distributions which Include MariaDB](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/distributions-which-include-mariadb/)
page.