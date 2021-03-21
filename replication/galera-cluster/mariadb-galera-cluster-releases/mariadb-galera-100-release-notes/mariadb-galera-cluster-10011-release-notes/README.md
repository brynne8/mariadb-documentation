# MariaDB Galera Cluster 10.0.11 Release Notes

The most recent [MariaDB Galera Cluster 10.0](/kb/en/galera/) release is:<br>
<span class="cstm-style lead"><strong>[MariaDB Galera Cluster 10.0.38](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10038-release-notes)</strong> [Download<span>&nbsp;</span>Now](https://downloads.mariadb.org/mariadb-galera/10.0)</span>

[Download](http://downloads.mariadb.org/mariadb-galera/10.0.11)
[Release Notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10011-release-notes)
[Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-changelogs/mariadb-galera-cluster-10011-changelog)
[Overview of MariaDB Galera Cluster](/replication/galera-cluster/what-is-mariadb-galera-cluster)

<strong>Release date:</strong> 11 Jun 2014

This is the 3<sup>rd</sup> release in the MariaDB Galera Cluster 10.0 series. It is a
<strong><em>[Beta](/kb/en/release-criteria/)</em></strong> release.  It is a merge of [MariaDB 10.0.10](/kb/en/mariadb-10010-release-notes/)
and [Galera Cluster](http://codership.com/content/using-galera-cluster) with
additional bug fixes. It is being released now to get it into the hands of any
who might want to test it. <strong>Do not run Beta releases on production systems!</strong>

Various articles about MariaDB Galera Cluster, including
[known limitations](/replication/galera-cluster/mariadb-galera-cluster-known-limitations) and
[how to get started](/replication/galera-cluster/getting-started-with-mariadb-galera-cluster) are
available in the <strong>[Galera](/kb/en/galera/)</strong> section of the Knowledge Base.

For a list of changes made in MariaDB Galera Cluster 10.0.11, with links to
detailed information on each push, see the
[MariaDB Galera Cluster 10.0.11 Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-changelogs/mariadb-galera-cluster-10011-changelog).

## Updates and fixes in this version

- Galera `garbd` and `libgalera` are now included in the binary tarballs ([MDEV-4463](https://jira.mariadb.org/browse/MDEV-4463))
- Codership changes: `lp:codership-mysql/5.5` (till rev `3991`) &amp; `lp:codership-mysql/5.6` (till rev `4091`).
- [Galera library](http://codership.com/content/using-galera-cluster) versions: 25.3.5, 25.2.9
- Supported wsrep interface API version: 25
- Fixes for the following security vulnerabilities:
<ul start="1"><li>[CVE-2014-2436](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-2436)
</li><li>[CVE-2014-2440](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-2440)
</li><li>[CVE-2014-2430](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-2430)
</li><li>[CVE-2014-2431](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-2431)
</li><li>[CVE-2019-2481](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-2481)
</li></ul>

## Notes

- Compatibility: Wsrep providers (Galera libraries) other than version 25.x.xx are not supported.
- See the [MariaDB 10.0.11 Release Notes](/kb/en/mariadb-10011-release-notes/) and
[Changelog](/kb/en/mariadb-10011-changelog/) for more information on the changes in
MariaDB.
- If Galera v2 and v3 are both being used in the cluster, MariaDB with
Galera v3 must be started with [wsrep_provider_options='socket.checksum=1'](/kb/en/wsrep_provider_options/#socketchecksum)
in order to make it backward compatible with Galera v2.

Thanks, and enjoy MariaDB Galera Cluster!