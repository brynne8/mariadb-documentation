# MariaDB Galera Cluster 10.0.13 Release Notes

The most recent [MariaDB Galera Cluster 10.0](/kb/en/galera/) release is:<br>
<span class="cstm-style lead"><strong>[MariaDB Galera Cluster 10.0.38](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10038-release-notes)</strong> [Download<span>&nbsp;</span>Now](https://downloads.mariadb.org/mariadb-galera/10.0)</span>

[Download](http://downloads.mariadb.org/mariadb-galera/10.0.13)
[Release Notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10013-release-notes)
[Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-changelogs/mariadb-galera-cluster-10013-changelog)
[Overview of MariaDB Galera Cluster](/replication/galera-cluster/what-is-mariadb-galera-cluster)

<strong>Release date:</strong> 3 Sep 2014

This is the 5<sup>th</sup> release in the MariaDB Galera Cluster 10.0 series. It is a
<strong><em>[Stable](/kb/en/release-criteria/)</em></strong> (GA) release.  It is a merge of [MariaDB 10.0.13](/kb/en/mariadb-10013-release-notes/)
and [Galera Cluster](http://codership.com/content/using-galera-cluster) with
additional bug fixes.

Various articles about MariaDB Galera Cluster, including
[known limitations](/replication/galera-cluster/mariadb-galera-cluster-known-limitations) and
[how to get started](/replication/galera-cluster/getting-started-with-mariadb-galera-cluster) are
available in the <strong>[Galera](/kb/en/galera/)</strong> section of the Knowledge Base.

For a list of changes made in MariaDB Galera Cluster 10.0.13, with links to
detailed information on each push, see the
[MariaDB Galera Cluster 10.0.13 Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-changelogs/mariadb-galera-cluster-10013-changelog).

## Updates and fixes in this version

- Galera `garbd` and `libgalera` are now included in the binary tarballs
  ([MDEV-4463](https://jira.mariadb.org/browse/MDEV-4463))

- Codership changes: `lp:codership-mysql/5.6` (till rev `4123`).

- [Galera library](http://codership.com/content/using-galera-cluster)
  versions: 25.3.5, 25.2.9

- Supported wsrep interface API version: 25

## Notes

- [Snapshot state transfer](/kb/en/galera-cluster-system-variables/#wsrep_sst_method): rsync and mysqldump SST methods support [GTID](/kb/en/global-transaction-id/). However, xtrabackup-v2
  and xtrabackup SST methods currently do not support GTID.
  ([lp:1326967](https://bugs.launchpad.net/percona-xtrabackup/+bug/1326967))

- Running MariaDB Galera Cluster 5.5 and 10.0 nodes in a cluster is not
  supported ([MDEV-6257](https://jira.mariadb.org/browse/MDEV-6257))

- Compatibility: Wsrep providers (Galera libraries) other than version 25.x.xx
  are not supported.

- Compatibility: If Galera v2 and v3 are both being used in the cluster, MariaDB with
  Galera v3 must be started with
[wsrep_provider_options='socket.checksum=1'](/kb/en/wsrep_provider_options/#socketchecksum)
  in order to make it backward compatible with Galera v2.

- See the [MariaDB 10.0.13 Release Notes](/kb/en/mariadb-10013-release-notes/) and
  [Changelog](/kb/en/mariadb-10013-changelog/) for more information on the changes in
  MariaDB.

Thanks, and enjoy MariaDB Galera Cluster!