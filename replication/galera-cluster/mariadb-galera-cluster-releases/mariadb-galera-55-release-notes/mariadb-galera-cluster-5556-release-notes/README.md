# MariaDB Galera Cluster 5.5.56 Release Notes

The most recent [MariaDB Galera Cluster 5.5](/kb/en/galera/) release is:<br>
<span class="cstm-style lead"><strong>[MariaDB Galera Cluster 5.5.63](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5563-release-notes)</strong> [Download<span>&nbsp;</span>Now](https://downloads.mariadb.org/mariadb-galera/5.5)</span>

[Download](http://downloads.mariadb.org/mariadb-galera/5.5.56)
[Release Notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5556-release-notes)
[Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-changelogs/mariadb-galera-cluster-5556-changelog)
[Overview of MariaDB Galera Cluster](/replication/galera-cluster/what-is-mariadb-galera-cluster)

<strong>Release date:</strong> 3 May 2017

MariaDB Galera Cluster 5.5.56 is a <strong><em>[Stable](/kb/en/release-criteria/)</em></strong> (GA)
release. It is a merge of [MariaDB 5.5.56](/kb/en/mariadb-5556-release-notes/) and
[Galera Cluster](http://codership.com/content/using-galera-cluster) with
additional bug fixes.

Various articles about MariaDB Galera Cluster, including
[known limitations](/replication/galera-cluster/mariadb-galera-cluster-known-limitations) and
[how to get started](/replication/galera-cluster/getting-started-with-mariadb-galera-cluster) are
available in the <strong>[Galera](/kb/en/galera/)</strong> section of the Knowledge Base.

## Updates and fixes in this version

- This release is a bug-fix release.

- Codership changes:
  [github.com/codership/mysql-wsrep/tree/5.5](https://github.com/codership/mysql-wsrep/tree/5.5)
  (till commit `9949137`)

- The [Galera library](http://codership.com/content/using-galera-cluster) used
  by MariaDB Galera Cluster and included in the MariaDB repositories is
  currently at version <strong>25.3.20</strong>.

- Fixes for the following [security vulnerabilities](/kb/en/cve/):
<ul start="1"><li>CVE-`<code>-`</code><code>
</code></li></ul>

See the [MariaDB 5.5.56 Release Notes](/kb/en/mariadb-5556-release-notes/) for more
information on fixes in this version.

## Changelog

A full list of all changes is in the
[MariaDB Galera Cluster 5.5.56 Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-changelogs/mariadb-galera-cluster-5556-changelog)
and the [MariaDB 5.5.56 Changelog](/kb/en/mariadb-5556-changelog/).

## Notes

- Running MariaDB Galera Cluster 5.5 and 10.0 nodes in a cluster is not
  supported ([MDEV-6257](https://jira.mariadb.org/browse/MDEV-6257))

- This version of MariaDB Galera Cluster supports `wsrep` API v25 which means
  MariaDB Galera Cluster can be used with either a 25.2.x or 25.3.x

---

Be notified of new MariaDB Server releases automatically by [subscribing](https://lists.askmonty.org/cgi-bin/mailman/listinfo/announce) to the MariaDB Foundation community announce 'at' mariadb.org announcement list (this is a low traffic, announce-only list). MariaDB Corporation customers will be notified for all new releases, security issues and critical bug fixes for all MariaDB Corporation products thanks to the Notification Services.
<br><br>
MariaDB may already be included in your favorite OS distribution. More
information can be found on the
[Distributions which Include MariaDB](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/distributions-which-include-mariadb)
page.