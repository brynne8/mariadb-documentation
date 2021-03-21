# MariaDB Galera Cluster 10.0.10 Release Notes

The most recent [MariaDB Galera Cluster 10.0](/kb/en/galera/) release is:<br>
<span class="cstm-style lead"><strong>[MariaDB Galera Cluster 10.0.38](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10038-release-notes)</strong> [Download<span>&nbsp;</span>Now](https://downloads.mariadb.org/mariadb-galera/10.0)</span>

[Download](http://downloads.mariadb.org/mariadb-galera/10.0.10) |
<strong>Release Notes</strong> |
[Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-changelogs/mariadb-galera-cluster-10010-changelog) |
[Overview of MariaDB Galera Cluster](/replication/galera-cluster/what-is-mariadb-galera-cluster)

<strong>Release date:</strong> 22 Apr 2014

This is the 2<sup>nd</sup> release in the MariaDB Galera Cluster 10.0 series. It is a
<strong><em>[Beta](/kb/en/release-criteria/)</em></strong> release.  It is a merge of [MariaDB 10.0.10](/kb/en/mariadb-10010-release-notes/)
and [Galera Cluster](http://codership.com/content/using-galera-cluster) with
additional bug fixes. It is being released now to get it into the hands of any
who might want to test it. <strong>Do not run Beta releases on production systems!</strong>

Various articles about MariaDB Galera Cluster, including
[known limitations](/replication/galera-cluster/mariadb-galera-cluster-known-limitations) and
[how to get started](/replication/galera-cluster/getting-started-with-mariadb-galera-cluster) are
available in the <strong>[Galera](/kb/en/galera/)</strong> section of the Knowledge Base.

For a list of changes made in MariaDB Galera Cluster 10.0.10, with links to
detailed information on each push, see the
[MariaDB Galera Cluster 10.0.10 Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-changelogs/mariadb-galera-cluster-10010-changelog).

## Updates and fixes in this version

- Codership changes: `lp:codership-mysql/5.5` (till rev `3968`) &amp; `lp:codership-mysql/5.6` (till rev `4065`).
- [Galera library](http://codership.com/content/using-galera-cluster) versions: 25.3.5, 25.2.9
- Supported wsrep interface version: 25

## Notes

- Compatibility: Wsrep providers (Galera libraries) other than version 25.x.xx are not supported.
- New: This release includes a new method for snapshot state transfer, `wsrep_sst_xtrabackup-v2`. This method of state snapshot transfer can be configured using the <code class="fixed" style="white-space:pre-wrap">--wsrep_sst_method=xtrabackup-v2</code> option. Its use requires Xtrabackup (&gt;= 2.1.6) and other software packages like socat, nc, and tar.
- Note: Completely uninstall broken/partially installed 10.0.7-galera deb packages prior to installing this version, as they may conflict with 10.0.10-galera packages.

See the [MariaDB 10.0.10 Release Notes](/kb/en/mariadb-10010-release-notes/) and
[Changelog](/kb/en/mariadb-10010-changelog/) for more information on the changes in
MariaDB.

Note: If Galera v2 and v3 are both being used in the cluster, MariaDB with
Galera v3 must be started with `wsrep_provider_options='socket.checksum=1'`
in order to make it backward compatible with Galera v2.

Thanks, and enjoy MariaDB Galera Cluster!

---

Be notified of new MariaDB Server releases automatically by [subscribing](https://lists.askmonty.org/cgi-bin/mailman/listinfo/announce) to the MariaDB Foundation community announce 'at' mariadb.org announcement list (this is a low traffic, announce-only list). MariaDB Corporation customers will be notified for all new releases, security issues and critical bug fixes for all MariaDB Corporation products thanks to the Notification Services.
<br><br>
MariaDB may already be included in your favorite OS distribution. More
information can be found on the
[Distributions which Include MariaDB](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/distributions-which-include-mariadb)
page.