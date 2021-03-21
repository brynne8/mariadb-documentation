# MariaDB Galera Cluster 10.0.7 Release Notes

The most recent [MariaDB Galera Cluster 10.0](/kb/en/galera/) release is:<br>
<span class="cstm-style lead"><strong>[MariaDB Galera Cluster 10.0.38](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10038-release-notes/)</strong> [Download<span>&nbsp;</span>Now](https://downloads.mariadb.org/mariadb-galera/10.0)</span>

[Download](http://downloads.mariadb.org/mariadb-galera/10.0.7) |
<strong>Release Notes</strong> |
[Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-changelogs/mariadb-galera-cluster-1007-changelog/) |
[Overview of MariaDB Galera Cluster](/replication/galera-cluster/what-is-mariadb-galera-cluster/)

<strong>Release date:</strong> 24 Feb 2014

MariaDB Galera Cluster 10.0.7 is an <strong><em>[Alpha](/kb/en/release-criteria/)</em></strong> release.
It is a merge of [MariaDB 10.0.7](/kb/en/what-is-mariadb-100/) and
[Galera Cluster](http://codership.com/content/using-galera-cluster) with
additional bug fixes.

Various articles about MariaDB Galera Cluster, including
[known limitations](/replication/galera-cluster/mariadb-galera-cluster-known-limitations/) and
[how to get started](/replication/galera-cluster/getting-started-with-mariadb-galera-cluster/) are
available in the <strong>[Galera](/kb/en/galera/)</strong> section of the Knowledge Base.

For a list of changes made in MariaDB Galera Cluster 10.0.7, with links to
detailed information on each push, see the
[MariaDB Galera Cluster 10.0.7 Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-changelogs/mariadb-galera-cluster-1007-changelog/).

## Includes [MariaDB 10.0.7](/kb/en/mariadb-1007-release-notes/) and Galera Cluster

MariaDB Galera Cluster 10.0.7 includes
[MariaDB 10.0.7](/kb/en/mariadb-1007-release-notes/) with Codership additions
(`lp:codership-mysql/5.5` till revision `3944`) and
[Galera 25.3.2](http://codership.com/content/using-galera-cluster).
This version of MariaDB Galera Cluster supports `wsrep` API v25 which means
MariaDB Galera Cluster can be used with either a 25.2.x or 25.3.x Galera
`wsrep` provider. A 25.3.x `wsrep` provider is included in the MariaDB
repositories. And both 25.3.x and 25.2.x `wsrep` providers are available on the downloads page.

See the [MariaDB 10.0.7 Release Notes](/kb/en/mariadb-1007-release-notes/) and
[Changelog](/kb/en/mariadb-1007-changelog/) for more information on the changes in
MariaDB.

Note: If Galera v2 and v3 are both being used in the cluster, MariaDB with Galera v3 must be started with wsrep_provider_options='socket.checksum=1' in order to make it backward compatible with Galera v2.

## First Alpha Release

This is the first alpha release for this series of MariaDB Galera Cluster. It
is being released now to get it into the hands of any who might want to test
it. <strong>Do not run Alpha releases on production systems!</strong>

Thanks, and enjoy MariaDB Galera Cluster!

---

Be notified of new MariaDB Server releases automatically by [subscribing](https://lists.askmonty.org/cgi-bin/mailman/listinfo/announce) to the MariaDB Foundation community announce 'at' mariadb.org announcement list (this is a low traffic, announce-only list). MariaDB Corporation customers will be notified for all new releases, security issues and critical bug fixes for all MariaDB Corporation products thanks to the Notification Services.
<br><br>
MariaDB may already be included in your favorite OS distribution. More
information can be found on the
[Distributions which Include MariaDB](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/distributions-which-include-mariadb/)
page.