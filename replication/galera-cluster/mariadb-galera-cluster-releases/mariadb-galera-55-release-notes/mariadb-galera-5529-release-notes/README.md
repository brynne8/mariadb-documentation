# MariaDB Galera 5.5.29 Release Notes

The most recent [MariaDB Galera Cluster 5.5](/kb/en/galera/) release is:<br>
<span class="cstm-style lead"><strong>[MariaDB Galera Cluster 5.5.63](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5563-release-notes)</strong> [Download<span>&nbsp;</span>Now](https://downloads.mariadb.org/mariadb-galera/5.5)</span>

[Download](http://downloads.mariadb.org/mariadb-galera/5.5.29) |
<strong>Release Notes</strong> |
[Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-changelogs/mariadb-galera-5529-changelog) |
[Overview of Galera](/replication/galera-cluster/what-is-mariadb-galera-cluster)

<strong>Release date:</strong> 5 Mar 2013

[MariaDB Galera 5.5.29](/kb/en/mariadb-galera-cluster-5529-release-notes/) is a <strong><em>[Stable](/kb/en/release-criteria/)</em></strong> (GA) release. In general this means that there are no known serious bugs, except for those marked as feature requests, that no bugs were fixed since last release that caused a notable code changes, and that we believe the code is ready for general usage (based on bug inflow). It is a merge of [MariaDB 5.5.29](/kb/en/what-is-mariadb-55/) and [Galera Cluster](http://codership.com/content/using-galera-cluster) with some additional bug fixes.

Various articles about MariaDB Galera Cluster, including
[known limitations](/replication/galera-cluster/mariadb-galera-cluster-known-limitations) and
[how to get started](/replication/galera-cluster/getting-started-with-mariadb-galera-cluster) are
available in the <strong>[Galera](/kb/en/galera/)</strong> section of the Knowledgebase.

For a list of changes made in [MariaDB Galera 5.5.29](/kb/en/mariadb-galera-cluster-5529-release-notes/), with links to detailed
information on each push, see the
[MariaDB Galera 5.5.29 Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-changelogs/mariadb-galera-5529-changelog).

In most respects [MariaDB](/replication/optimization-and-tuning/query-optimizations/guiduuid-performance/mariadb) will work exactly as MySQL: all commands,
interfaces, libraries and APIs that exist in MySQL also exist in MariaDB.

## Includes [MariaDB 5.5.29](/kb/en/mariadb-5529-release-notes/) and Galera Cluster

[MariaDB Galera 5.5.29](/kb/en/mariadb-galera-cluster-5529-release-notes/) includes [MariaDB 5.5.29](/kb/en/mariadb-5529-release-notes/) and
[Galera Cluster](http://codership.com/content/using-galera-cluster). See the
[MariaDB 5.5.29](/kb/en/mariadb-5529-release-notes/) [Release Notes](/kb/en/mariadb-5529-release-notes/) and
[Changelog](/kb/en/mariadb-5529-changelog/) for more information on the changes in
[MariaDB 5.5.29](/kb/en/mariadb-5529-release-notes/).

## Combined Repositories

The repositories for MariaDB and MariaDB Galera Cluster are combined. Please
visit the
[MariaDB Repository Configuration Tool](https://downloads.mariadb.org/mariadb/repositories/)
to generate commands suitable for many different Linux Distributions.

When using the repositories to install MariaDB Galera Cluster, the only
difference between it and installing MariaDB is to specify the MariaDB Galera
Server package instead of the MariaDB Server package and to install the Galera
package. For example, on Ubuntu, after updating the mariadb.list file the
following command will install MariaDB Galera Server:

```sql
sudo apt-get update
sudo apt-get install mariadb-galera-server galera
```

If MariaDB is already installed on the server the package manager will
uninstall the appropriate packages prior to installing the MariaDB Galera
Cluster packages.

See the [MariaDB 5.5.29 Changelog](/kb/en/mariadb-5529-changelog/) for full details
of the many packaging and other fixes.

Thanks, and enjoy MariaDB Galera Cluster!

---

Be notified of new MariaDB Server releases automatically by [subscribing](https://lists.askmonty.org/cgi-bin/mailman/listinfo/announce) to the MariaDB Foundation community announce 'at' mariadb.org announcement list (this is a low traffic, announce-only list). MariaDB Corporation customers will be notified for all new releases, security issues and critical bug fixes for all MariaDB Corporation products thanks to the Notification Services.
<br><br>
MariaDB may already be included in your favorite OS distribution. More
information can be found on the
[Distributions which Include MariaDB](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/distributions-which-include-mariadb)
page.