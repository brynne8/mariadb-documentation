# MariaDB Galera Cluster 5.5.37 Release Notes

The most recent [MariaDB Galera Cluster 5.5](/kb/en/galera/) release is:<br>
<span class="cstm-style lead"><strong>[MariaDB Galera Cluster 5.5.63](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5563-release-notes)</strong> [Download<span>&nbsp;</span>Now](https://downloads.mariadb.org/mariadb-galera/5.5)</span>

[Download](http://downloads.mariadb.org/mariadb-galera/5.5.37) |
<strong>Release Notes</strong> |
[Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-changelogs/mariadb-galera-cluster-5537-changelog) |
[Overview of MariaDB Galera Cluster](/replication/galera-cluster/what-is-mariadb-galera-cluster)

<strong>Release date:</strong> 30 Apr 2014

MariaDB Galera Cluster 5.5.37 is a <strong><em>[Stable](/kb/en/release-criteria/)</em></strong> (GA) release.
It is a merge of [MariaDB 5.5.37](/kb/en/mariadb-5537-release-notes/) and
[Galera Cluster](http://codership.com/content/using-galera-cluster) with
additional bug fixes.

Various articles about MariaDB Galera Cluster, including
[known limitations](/replication/galera-cluster/mariadb-galera-cluster-known-limitations) and
[how to get started](/replication/galera-cluster/getting-started-with-mariadb-galera-cluster) are
available in the <strong>[Galera](/kb/en/galera/)</strong> section of the Knowledge Base.

For a list of changes made in MariaDB Galera Cluster 5.5.37, with links to detailed
information on each push, see the
[MariaDB Galera Cluster 5.5.37 Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-changelogs/mariadb-galera-cluster-5537-changelog).

## Notes about this release

- MariaDB Galera Cluster 5.5.37 includes [MariaDB 5.5.37](/kb/en/mariadb-5537-release-notes/) with Codership
  additions (`lp:codership-mysql/5.5` till revision `3980`) and
  [Galera 25.3.5](http://codership.com/content/using-galera-cluster).

- This version of MariaDB Galera Cluster supports `wsrep` API v25 which means
  MariaDB Galera Cluster can be used with either a 25.2.x or 25.3.x
  Galera `wsrep` provider. A 25.3.x `wsrep` provider is included in the
  MariaDB repositories and both 25.3.x and 25.2.x wsrep providers are available
  on the downloads page.

- This release includes a new method for snapshot state
  transfer, `wsrep_sst_xtrabackup-v2`. This method of state snapshot transfer
  can be configured using the <code class="fixed" style="white-space:pre-wrap">--wsrep_sst_method=xtrabackup-v2</code>
  option. Its use requires [Percona Xtrabackup](/kb/en/percona-xtrabackup/) (&gt;= 2.1.6) and other software packages
  like socat, nc, and tar.

See the [MariaDB 5.5.37 Release Notes](/kb/en/mariadb-5537-release-notes/) and
[Changelog](/kb/en/mariadb-5537-changelog/) for more information on the changes in
MariaDB.

Note: If Galera 25.2.x and 25.3.x are both being used in the cluster, MariaDB
with Galera 25.3.x must be started with
`wsrep_provider_options='socket.checksum=1'` in order to make it backward
compatible with Galera v2. Galera wsrep providers other than 25.3.x or 25.2.x
are not supported.

Important: wsrep_sst_rsync (sst script) in MariaDB Galera Cluster 5.5.37 uses `lsof` which might not be installed by default on some linux distributions. It has to be installed if the `wsrep_sst_method` is set to `rsync` (default). Future MariaDB Galera Cluster releases will address this by adding a dependency on lsof.

## Bug Fixes

This release contains fixes for bugs, compiler errors/warnings and improvements
in existing scripts.

A list of all the bugs fixed can be found in the [changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-changelogs/mariadb-galera-cluster-5537-changelog).

- Fixes for the following security vulnerabilities:
<ul start="1"><li>[CVE-2014-2436](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-2436)
</li><li>[CVE-2014-2440](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-2440)
</li><li>[CVE-2014-2430](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-2430)
</li><li>[CVE-2014-2431](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-2431)
</li><li>[CVE-2019-2481](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-2481)
</li></ul>

Thanks, and enjoy MariaDB Galera Cluster!

---

Be notified of new MariaDB Server releases automatically by [subscribing](https://lists.askmonty.org/cgi-bin/mailman/listinfo/announce) to the MariaDB Foundation community announce 'at' mariadb.org announcement list (this is a low traffic, announce-only list). MariaDB Corporation customers will be notified for all new releases, security issues and critical bug fixes for all MariaDB Corporation products thanks to the Notification Services.
<br><br>
MariaDB may already be included in your favorite OS distribution. More
information can be found on the
[Distributions which Include MariaDB](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/distributions-which-include-mariadb)
page.