# MariaDB Galera Cluster 5.5.36 Release Notes

The most recent [MariaDB Galera Cluster 5.5](/kb/en/galera/) release is:<br>
<span class="cstm-style lead"><strong>[MariaDB Galera Cluster 5.5.63](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5563-release-notes/)</strong> [Download<span>&nbsp;</span>Now](https://downloads.mariadb.org/mariadb-galera/5.5)</span>

[Download](http://downloads.mariadb.org/mariadb-galera/5.5.36) |
<strong>Release Notes</strong> |
[Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-changelogs/mariadb-galera-cluster-5536-changelog/) |
[Overview of MariaDB Galera Cluster](/replication/galera-cluster/what-is-mariadb-galera-cluster/)

<strong>Release date:</strong> 12 Mar 2014

MariaDB Galera Cluster 5.5.36 is a <strong><em>[Stable](/kb/en/release-criteria/)</em></strong> (GA) release.
It is a merge of [MariaDB 5.5.36](/kb/en/mariadb-5536-release-notes/) and
[Galera Cluster](http://codership.com/content/using-galera-cluster) with
additional bug fixes.

Various articles about MariaDB Galera Cluster, including
[known limitations](/replication/galera-cluster/mariadb-galera-cluster-known-limitations/) and
[how to get started](/replication/galera-cluster/getting-started-with-mariadb-galera-cluster/) are
available in the <strong>[Galera](/kb/en/galera/)</strong> section of the Knowledge Base.

For a list of changes made in MariaDB Galera Cluster 5.5.36, with links to detailed
information on each push, see the
[MariaDB Galera Cluster 5.5.36 Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-changelogs/mariadb-galera-cluster-5536-changelog/).

## Includes [MariaDB 5.5.36](/kb/en/mariadb-5536-release-notes/) and Galera

MariaDB Galera Cluster 5.5.36 includes [MariaDB 5.5.36](/kb/en/mariadb-5536-release-notes/) with Codership additions
(`lp:codership-mysql/5.5` till revision `3944`) and
[Galera 25.3.2](http://codership.com/content/using-galera-cluster).
This version of MariaDB Galera Cluster supports `wsrep` API v25 which means
MariaDB Galera Cluster can be used with either a 25.2.x or 25.3.x Galera
`wsrep` provider. A 25.3.x `wsrep` provider is included in the MariaDB
repositories and both 25.3.x and 25.2.x wsrep providers are available on the downloads page.

See the [MariaDB 5.5.36 Release Notes](/kb/en/mariadb-5536-release-notes/) and
[Changelog](/kb/en/mariadb-5536-changelog/) for more information on the changes in
MariaDB.

Important : While upgrading the nodes of a MariaDB Galera cluster care must be
taken when using different versions of Galera library (wsrep provider). A
MariaDB Galera cluster node with Galera version 3 (25.3.xx) might fail to join
an existing cluster running with Galera 2 (25.2.xx) nodes. This happens due to
difference in the default values of 'socket.checksum' for Galera 2 and Galera 3
libraries.<br><br>
So, in order to fix this, the node with Galera 3 should be started
with an additional wsrep provider option: `socket.checksum=1` in
order to make it backward compatible with nodes running with Galera 2.<br><br>
For example:

```sql
--wsrep_provider_options='socket.checksum=1; [other provider options; ...]'
```

## Bug Fixes

This release contains fixes for bugs, compiler errors/warnings and improvements
in existing scripts.

A list of all the bugs fixed can be found in the [changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-changelogs/mariadb-galera-cluster-5536-changelog/).

Thanks, and enjoy MariaDB Galera Cluster!

---

Be notified of new MariaDB Server releases automatically by [subscribing](https://lists.askmonty.org/cgi-bin/mailman/listinfo/announce) to the MariaDB Foundation community announce 'at' mariadb.org announcement list (this is a low traffic, announce-only list). MariaDB Corporation customers will be notified for all new releases, security issues and critical bug fixes for all MariaDB Corporation products thanks to the Notification Services.
<br><br>
MariaDB may already be included in your favorite OS distribution. More
information can be found on the
[Distributions which Include MariaDB](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/distributions-which-include-mariadb/)
page.