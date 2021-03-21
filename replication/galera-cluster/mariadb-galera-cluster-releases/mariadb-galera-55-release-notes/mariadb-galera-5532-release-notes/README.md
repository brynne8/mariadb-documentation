# MariaDB Galera 5.5.32 Release Notes

The most recent [MariaDB Galera Cluster 5.5](/kb/en/galera/) release is:<br>
<span class="cstm-style lead"><strong>[MariaDB Galera Cluster 5.5.63](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5563-release-notes/)</strong> [Download<span>&nbsp;</span>Now](https://downloads.mariadb.org/mariadb-galera/5.5)</span>

[Download](http://downloads.mariadb.org/mariadb-galera/5.5.32) |
<strong>Release Notes</strong> |
[Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-changelogs/mariadb-galera-5532-changelog/) |
[Overview of Galera](/replication/galera-cluster/what-is-mariadb-galera-cluster/)

<strong>Release date:</strong> 30 Aug 2013

[MariaDB Galera 5.5.32](/kb/en/mariadb-galera-cluster-5532-release-notes/) is a <strong><em>[Stable](/kb/en/release-criteria/)</em></strong> (GA) release.
It is a merge of [MariaDB 5.5.32](/kb/en/what-is-mariadb-55/) and
[Galera Cluster](http://codership.com/content/using-galera-cluster) with
additional bug fixes.

Various articles about MariaDB Galera Cluster, including
[known limitations](/replication/galera-cluster/mariadb-galera-cluster-known-limitations/) and
[how to get started](/replication/galera-cluster/getting-started-with-mariadb-galera-cluster/) are
available in the <strong>[Galera](/kb/en/galera/)</strong> section of the Knowledgebase.

For a list of changes made in [MariaDB Galera 5.5.32](/kb/en/mariadb-galera-cluster-5532-release-notes/), with links to detailed
information on each push, see the
[MariaDB Galera 5.5.32 Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-changelogs/mariadb-galera-5532-changelog/).

## Includes [MariaDB 5.5.32](/kb/en/mariadb-5532-release-notes/) and Galera Cluster

[MariaDB Galera 5.5.32](/kb/en/mariadb-galera-cluster-5532-release-notes/) includes [MariaDB 5.5.32](/kb/en/mariadb-5532-release-notes/) and
[Galera Cluster](http://codership.com/content/using-galera-cluster). See the
[MariaDB 5.5.32](/kb/en/mariadb-5532-release-notes/) [Release Notes](/kb/en/mariadb-5532-release-notes/) and
[Changelog](/kb/en/mariadb-5532-changelog/) for more information on the changes in
[MariaDB 5.5.32](/kb/en/mariadb-5532-release-notes/).

## Includes Galera wsrep provider version 23.2.6

The Galera library on Ubuntu/Debian, and x86_64 versions of Red Hat 6, CentOS
6, and Fedora has been updated to version 23.2.6.

## Packaging Fixes

This version includes several packaging fixes including a fix for [MDEV-4229](https://jira.mariadb.org/browse/MDEV-4229)
regarding binaries for Debian Wheezy.

One packaging fix that still exists is one that prevents the installation of
the wsrep package on Fedora systems ([MDEV-4141](https://jira.mariadb.org/browse/MDEV-4141)). It is hoped this issue will be fixed soon. When it is this paragraph will be updated. For now, Fedora packages are in the repository, but the galera package must be installed manually, and may not work even then.

## Other fixes

- [MDEV-4953](https://jira.mariadb.org/browse/MDEV-4953) Delete on a partioned table is not replicated
- LOAD DATA INFILE now supports big data files by introducing transaction
  splitting, which is controlled via the [wsrep_load_data_splitting](/kb/en/mariadb-galera-cluster-configuration-variables/#wsrep_load_data_splitting) global
  variable

Thanks, and enjoy MariaDB Galera Cluster!

---

Be notified of new MariaDB Server releases automatically by [subscribing](https://lists.askmonty.org/cgi-bin/mailman/listinfo/announce) to the MariaDB Foundation community announce 'at' mariadb.org announcement list (this is a low traffic, announce-only list). MariaDB Corporation customers will be notified for all new releases, security issues and critical bug fixes for all MariaDB Corporation products thanks to the Notification Services.
<br><br>
MariaDB may already be included in your favorite OS distribution. More
information can be found on the
[Distributions which Include MariaDB](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/distributions-which-include-mariadb/)
page.