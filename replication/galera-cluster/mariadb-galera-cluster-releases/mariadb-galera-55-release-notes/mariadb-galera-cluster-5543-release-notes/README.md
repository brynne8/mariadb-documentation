# MariaDB Galera Cluster 5.5.43 Release Notes

The most recent [MariaDB Galera Cluster 5.5](/kb/en/galera/) release is:<br>
<span class="cstm-style lead"><strong>[MariaDB Galera Cluster 5.5.63](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5563-release-notes/)</strong> [Download<span>&nbsp;</span>Now](https://downloads.mariadb.org/mariadb-galera/5.5)</span>

[Download](http://downloads.mariadb.org/mariadb-galera/5.5.43)
[Release Notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5543-release-notes/)
[Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-changelogs/mariadb-galera-cluster-5543-changelog/)
[Overview of MariaDB Galera Cluster](/replication/galera-cluster/what-is-mariadb-galera-cluster/)

<strong>Release date:</strong> 15 May 2015

MariaDB Galera Cluster 5.5.43 is a <strong><em>[Stable](/kb/en/release-criteria/)</em></strong> (GA) release.
It is a merge of [MariaDB 5.5.43](/kb/en/mariadb-5543-release-notes/) and
[Galera Cluster](http://codership.com/content/using-galera-cluster) with
additional bug fixes.

Various articles about MariaDB Galera Cluster, including
[known limitations](/replication/galera-cluster/mariadb-galera-cluster-known-limitations/) and
[how to get started](/replication/galera-cluster/getting-started-with-mariadb-galera-cluster/) are
available in the <strong>[Galera](/kb/en/galera/)</strong> section of the Knowledge Base.

For a list of changes made in MariaDB Galera Cluster 5.5.43, with links to detailed
information on each push, see the
[MariaDB Galera Cluster 5.5.43 Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-changelogs/mariadb-galera-cluster-5543-changelog/).

## Updates and fixes in this version

- Codership changes: `github.com/codership/mysql-wsrep/tree/5.5` (till commit `11dc27f`)

- Fix for [MDEV-8115](https://jira.mariadb.org/browse/MDEV-8115): mysql_upgrade crashes the server with REPAIR VIEW

- The following [FLUSH](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/) commands are now executed under total order isolation:
<ul start="1"><li>FLUSH DES_KEY_FILE
</li><li>FLUSH HOSTS
</li><li>FLUSH PRIVILEGES
</li><li>FLUSH QUERY CACHE
</li><li>FLUSH STATUS
</li><li>FLUSH USER_RESOURCES
</li></ul>

### Deprecated Distributions

As per the [MariaDB Deprecation Policy](/kb/en/mariadb-deprecation-policy/), this will
be the final release of MariaDB Galera Cluster 5.5 for Fedora 19 "Schrödinger's Cat", Ubuntu
10.04 LTS "Lucid", and Mint 9 LTS "Isadora". When the next
version of MariaDB Galera Cluster 5.5 is released, repositories for these distributions will
go away.

## Notes

- Running MariaDB Galera Cluster 5.5 and 10.0 nodes in a cluster is not
  supported ([MDEV-6257](https://jira.mariadb.org/browse/MDEV-6257))

- This version of MariaDB Galera Cluster supports `wsrep` API v25 which means
  MariaDB Galera Cluster can be used with either a 25.2.x or 25.3.x
  Galera `wsrep` provider. A 25.3.x `wsrep` provider is included in the
  MariaDB repositories and is also available from the
  [downloads](http://downloads.mariadb.org/mariadb-galera/5.5.43) page.

- See the [MariaDB 5.5.43 Release Notes](/kb/en/mariadb-5543-release-notes/) and
[Changelog](/kb/en/mariadb-5543-changelog/) for more information on the changes in
MariaDB.

Note: If Galera 25.2.x and 25.3.x are both being used in the cluster, MariaDB
with Galera 25.3.x must be started with
[wsrep_provider_options='socket.checksum=1'](/kb/en/wsrep_provider_options/#socketchecksum) in order to make it backward
compatible with Galera v2. Galera wsrep providers other than 25.3.x or 25.2.x
are not supported.

Thanks, and enjoy MariaDB Galera Cluster!

---

Be notified of new MariaDB Server releases automatically by [subscribing](https://lists.askmonty.org/cgi-bin/mailman/listinfo/announce) to the MariaDB Foundation community announce 'at' mariadb.org announcement list (this is a low traffic, announce-only list). MariaDB Corporation customers will be notified for all new releases, security issues and critical bug fixes for all MariaDB Corporation products thanks to the Notification Services.
<br><br>
MariaDB may already be included in your favorite OS distribution. More
information can be found on the
[Distributions which Include MariaDB](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/distributions-which-include-mariadb/)
page.