# MariaDB Galera 5.5.28a Release Notes

The most recent [MariaDB Galera Cluster 5.5](/kb/en/galera/) release is:<br>
<span class="cstm-style lead"><strong>[MariaDB Galera Cluster 5.5.63](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5563-release-notes)</strong> [Download<span>&nbsp;</span>Now](https://downloads.mariadb.org/mariadb-galera/5.5)</span>

[Download](http://downloads.mariadb.org/mariadb-galera/5.5.28a) |
<strong>Release Notes</strong> |
[Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-changelogs/mariadb-galera-5528a-changelog) |
[Overview of Galera](/replication/galera-cluster/what-is-mariadb-galera-cluster)

<strong>Release date:</strong> 21 Dec 2012

[MariaDB Galera 5.5.28](/kb/en/mariadb-galera-cluster-5528-release-notes/)a is a <strong><em>[Release Candidate](/kb/en/release-criteria/)</em></strong> (RC) release. It is
a merge of [MariaDB 5.5.28a](/kb/en/what-is-mariadb-55/) and Galera Cluster with some
additional bug fixes.

Various articles about MariaDB Galera Cluster, including
[known limitations](/replication/galera-cluster/mariadb-galera-cluster-known-limitations) and
[how to get started](/replication/galera-cluster/getting-started-with-mariadb-galera-cluster) are
available in the <strong>[Galera](/kb/en/galera/)</strong> section of the Knowledgebase.

For a list of changes made in [MariaDB Galera 5.5.28](/kb/en/mariadb-galera-cluster-5528-release-notes/)a, with links to detailed
information on each push, see the
[MariaDB Galera 5.5.28a Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-changelogs/mariadb-galera-5528a-changelog).

In most respects [MariaDB](/replication/optimization-and-tuning/query-optimizations/guiduuid-performance/mariadb) will work exactly as MySQL: all commands,
interfaces, libraries and APIs that exist in MySQL also exist in MariaDB.

## Includes [MariaDB 5.5.28](/kb/en/mariadb-5528-release-notes/)a and Galera Cluster

[MariaDB Galera 5.5.28](/kb/en/mariadb-galera-cluster-5528-release-notes/)a includes [MariaDB 5.5.28](/kb/en/mariadb-5528-release-notes/)a and
[Galera Cluster](http://codership.com/content/using-galera-cluster). See the
[MariaDB 5.5.28](/kb/en/mariadb-5528-release-notes/)a [Release Notes](/kb/en/mariadb-5528a-release-notes/) and
[Changelog](/kb/en/mariadb-5528a-changelog/) for more information on the changes in
[MariaDB 5.5.28](/kb/en/mariadb-5528-release-notes/)a.

## Packaging Fixes

Numerous packaging fixes have been incorporated into this release of MariaDB
Galera Cluster. In addition to the previously available RedHat, CentOS, and
Fedora packages, Ubuntu and Debian packages are now available.

## Combined Repositories

The repositories for MariaDB and MariaDB Galera Cluster are now combined. The
old MariaDB Galera Cluster Repository is being updated for this release but
will go away in the near future, so if you are using that repository, please
visit the
[MariaDB Repository Configuration Tool](https://downloads.mariadb.org/mariadb/repositories/)
and update your repository configuration file. If you feel comfortable editing
your existing repo config file the change is pretty easy, just replace
'5.5-galera' with '5.5'.

When using the repositories to install MariaDB Galera Cluster, the only
difference between it and installing MariaDB is to specify the MariaDB Galera
Server package instead of the MariaDB Server package and to install the Galera
package. For example, on Ubuntu, after updating the mariadb.list file the
following command will install MariaDB Galera Server:

```sql
sudo apt-get update
sudo apt-get install mariadb-galera-server
```

If MariaDB is already installed on the server the package manager will
uninstall the appropriate packages prior to installing the MariaDB Galera
Cluster packages.

See the [MariaDB 5.5.28a Changelog](/kb/en/mariadb-5528a-changelog/) for full details
of the many packaging and other fixes.

Thanks, and enjoy MariaDB Galera Cluster!

---

Be notified of new MariaDB Server releases automatically by [subscribing](https://lists.askmonty.org/cgi-bin/mailman/listinfo/announce) to the MariaDB Foundation community announce 'at' mariadb.org announcement list (this is a low traffic, announce-only list). MariaDB Corporation customers will be notified for all new releases, security issues and critical bug fixes for all MariaDB Corporation products thanks to the Notification Services.
<br><br>
MariaDB may already be included in your favorite OS distribution. More
information can be found on the
[Distributions which Include MariaDB](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/distributions-which-include-mariadb)
page.