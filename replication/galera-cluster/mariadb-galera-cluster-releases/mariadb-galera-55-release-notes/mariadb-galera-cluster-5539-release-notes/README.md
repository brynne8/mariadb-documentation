# MariaDB Galera Cluster 5.5.39 Release Notes

The most recent [MariaDB Galera Cluster 5.5](/kb/en/galera/) release is:<br>
<span class="cstm-style lead"><strong>[MariaDB Galera Cluster 5.5.63](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5563-release-notes)</strong> [Download<span>&nbsp;</span>Now](https://downloads.mariadb.org/mariadb-galera/5.5)</span>

[Download](http://downloads.mariadb.org/mariadb-galera/5.5.39)
[Release Notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5539-release-notes)
[Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-changelogs/mariadb-galera-cluster-5539-changelog)
[Overview of MariaDB Galera Cluster](/replication/galera-cluster/what-is-mariadb-galera-cluster)

<strong>Release date:</strong> 21 Aug 2014

MariaDB Galera Cluster 5.5.39 is a <strong><em>[Stable](/kb/en/release-criteria/)</em></strong> (GA) release.
It is a merge of [MariaDB 5.5.39](/kb/en/mariadb-5539-release-notes/) and
[Galera Cluster](http://codership.com/content/using-galera-cluster) with
additional bug fixes.

Various articles about MariaDB Galera Cluster, including
[known limitations](/replication/galera-cluster/mariadb-galera-cluster-known-limitations) and
[how to get started](/replication/galera-cluster/getting-started-with-mariadb-galera-cluster) are
available in the <strong>[Galera](/kb/en/galera/)</strong> section of the Knowledge Base.

For a list of changes made in MariaDB Galera Cluster 5.5.39, with links to detailed
information on each push, see the
[MariaDB Galera Cluster 5.5.39 Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-changelogs/mariadb-galera-cluster-5539-changelog).

## Notes about this release

- MariaDB Galera Cluster 5.5.39 includes [MariaDB 5.5.39](/kb/en/mariadb-5539-release-notes/) with Codership
  additions (`lp:codership-mysql/5.5` till revision `4014`) and
  [Galera 25.3.5](http://codership.com/content/using-galera-cluster).

- This version of MariaDB Galera Cluster supports `wsrep` API v25 which means
  MariaDB Galera Cluster can be used with either a 25.2.x or 25.3.x
  Galera `wsrep` provider. A 25.3.x `wsrep` provider is included in the
  MariaDB repositories and both 25.3.x and 25.2.x wsrep providers are available
  on the [downloads](http://downloads.mariadb.org/mariadb-galera/5.5.39) page.

- wsrep_sst_rsync (sst script) in MariaDB Galera Cluster 5.5.39 uses `lsof`
  which might not be installed by default on some linux distributions. It has
  to be installed if the [wsrep_sst_method](/kb/en/galera-cluster-system-variables/#wsrep_sst_method) is set to `rsync` (default).
  Package managers should be able to automatically resolve this dependency
  ([MDEV-6264](https://jira.mariadb.org/browse/MDEV-6264)).

- There are now <em>test</em> packages available for MariaDB Galera Server in the
  YUM and APT
  [repositories](https://downloads.mariadb.org/mariadb/repositories/).  On
  Debian and Ubuntu, the package is called `mariadb-galera-test`. On Red Hat,
  CentOS, and Fedora the package is called `MariaDB-Galera-test`. These
  packages contain MTR framework and test suites along with some additional
  galera-specific suites which can be used for general testing of your MariaDB
  Galera Cluster installation, and for development.

See the [MariaDB 5.5.39 Release Notes](/kb/en/mariadb-5539-release-notes/) and
[Changelog](/kb/en/mariadb-5539-changelog/) for more information on the changes in
MariaDB.

Note: If Galera 25.2.x and 25.3.x are both being used in the cluster, MariaDB
with Galera 25.3.x must be started with
[wsrep_provider_options='socket.checksum=1'](/kb/en/wsrep_provider_options/#socketchecksum) in order to make it backward
compatible with Galera v2. Galera wsrep providers other than 25.3.x or 25.2.x
are not supported.

## Bug Fixes

This release contains fixes for bugs, compiler errors/warnings and improvements
in existing scripts.

A list of all the bugs fixed can be found in the
[changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-changelogs/mariadb-galera-cluster-5539-changelog).

Thanks, and enjoy MariaDB Galera Cluster!

---

Be notified of new MariaDB Server releases automatically by [subscribing](https://lists.askmonty.org/cgi-bin/mailman/listinfo/announce) to the MariaDB Foundation community announce 'at' mariadb.org announcement list (this is a low traffic, announce-only list). MariaDB Corporation customers will be notified for all new releases, security issues and critical bug fixes for all MariaDB Corporation products thanks to the Notification Services.
<br><br>
MariaDB may already be included in your favorite OS distribution. More
information can be found on the
[Distributions which Include MariaDB](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/distributions-which-include-mariadb)
page.