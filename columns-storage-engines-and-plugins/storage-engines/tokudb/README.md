# TokuDB

TokuDB has been deprecated by its upstream maintainer. It is disabled from [MariaDB 10.5](/kb/en/what-is-mariadb-105/) and has been been removed in [MariaDB 10.6](/kb/en/what-is-mariadb-106/) - [MDEV-19780](https://jira.mariadb.org/browse/MDEV-19780). We recommend [MyRocks](/columns-storage-engines-and-plugins/storage-engines/myrocks/) as a long-term migration path.

The TokuDB storage engine is for use in high-performance and write-intensive environments, offering increased compression and better performance.

It is available in an open-source version, included with 64-bit MariaDB (but not enabled by default), and an Enterprise edition available from Tokutek.

Official TokuDB Product Specs and Manuals are available on the Percona website. See:

- [TokuDB Documentation](https://www.percona.com/doc/percona-tokudb/index.html)

TokuDB is available on the following distributions:

<table><tbody><tr><th>Distribution</th><th>Introduced</th></tr>
<tr><td>CentOS 6 64-bit and newer</td><td><a href="/kb/en/mariadb-5536-release-notes/">MariaDB 5.5.36</a> and <a href="/kb/en/mariadb-1009-release-notes/">MariaDB 10.0.9</a></td></tr>
<tr><td>Debian 7 "wheezy"64-bit and newer</td><td><a href="/kb/en/mariadb-5533-release-notes/">MariaDB 5.5.33</a> and <a href="/kb/en/mariadb-1005-release-notes/">MariaDB 10.0.5</a></td></tr>
<tr><td>Fedora 19 64-bit and newer</td><td><a href="/kb/en/mariadb-5533-release-notes/">MariaDB 5.5.33</a> and <a href="/kb/en/mariadb-1005-release-notes/">MariaDB 10.0.5</a></td></tr>
<tr><td>openSUSE 13.1 64-bit and newer</td><td><a href="/kb/en/mariadb-5541-release-notes/">MariaDB 5.5.41</a> and <a href="/kb/en/mariadb-10015-release-notes/">MariaDB 10.0.15</a></td></tr>
<tr><td>Red Hat 6 64-bit and newer</td><td><a href="/kb/en/mariadb-5536-release-notes/">MariaDB 5.5.36</a> and <a href="/kb/en/mariadb-1009-release-notes/">MariaDB 10.0.9</a></td></tr>
<tr><td>Ubuntu 12.10 "quantal" 64-bit and newer</td><td><a href="/kb/en/mariadb-5533-release-notes/">MariaDB 5.5.33</a> and <a href="/kb/en/mariadb-1005-release-notes/">MariaDB 10.0.5</a></td></tr>
</tbody></table>

Note that the default value of [tokudb_pk_insert_mode](/kb/en/tokudb-system-variables/#tokudb_pk_insert_mode) will prevent row-based replication from working. To use RBR, change the value of this variable.

### Versions of the TokuDB plugin included in MariaDB

<table><tbody><tr><th>TokuDB Version</th><th>Introduced</th><th>Maturity</th></tr>
<tr><td>TokuDB from <a href="https://www.percona.com/doc/percona-server/5.6/release-notes/Percona-Server-5.6.49-89.0.html">Percona Server 5.6.49-89.0</a></td><td><a href="/kb/en/mariadb-10146-release-notes/">MariaDB 10.1.46</a></td><td>Stable</td></tr>
<tr><td>TokuDB from <a href="https://www.percona.com/doc/percona-server/5.6/release-notes/Percona-Server-5.6.46-86.2.html">Percona Server 5.6.46-86.2</a></td><td><a href="/kb/en/mariadb-10144-release-notes/">MariaDB 10.1.44</a></td><td>Stable</td></tr>
<tr><td>TokuDB from <a href="https://www.percona.com/doc/percona-server/5.6/release-notes/Percona-Server-5.6.43-84.3.html">Percona Server 5.6.43-84.3</a></td><td><a href="/kb/en/mariadb-10139-release-notes/">MariaDB 10.1.39</a></td><td>Stable</td></tr>
<tr><td>TokuDB from <a href="https://www.percona.com/doc/percona-server/5.6/release-notes/Percona-Server-5.6.42-84.2.html">Percona Server 5.6.42-84.2</a></td><td><a href="/kb/en/mariadb-10038-release-notes/">MariaDB 10.0.38</a></td><td>Stable</td></tr>
<tr><td>TokuDB from <a href="https://www.percona.com/doc/percona-server/5.6/release-notes/Percona-Server-5.6.41-84.1.html">Percona Server 5.6.41-84.1</a></td><td><a href="/kb/en/mariadb-10136-release-notes/">MariaDB 10.1.36</a>, <a href="/kb/en/mariadb-10037-release-notes/">MariaDB 10.0.37</a></td><td>Stable</td></tr>
<tr><td>TokuDB from <a href="https://www.percona.com/doc/percona-server/5.6/release-notes/Percona-Server-5.6.39-82.1.html">Percona Server 5.6.39-83.1</a></td><td><a href="/kb/en/mariadb-10035-release-notes/">MariaDB 10.0.35</a></td><td>Stable</td></tr>
<tr><td>TokuDB from <a href="https://www.percona.com/doc/percona-server/5.6/release-notes/Percona-Server-5.6.38-83.0.html">Percona Server 5.6.38-83.0</a></td><td><a href="/kb/en/mariadb-10131-release-notes/">MariaDB 10.1.31</a>, <a href="/kb/en/mariadb-10034-release-notes/">MariaDB 10.0.34</a></td><td>Stable</td></tr>
<tr><td>TokuDB from <a href="https://www.percona.com/doc/percona-server/5.6/release-notes/Percona-Server-5.6.37-82.2.html">Percona Server 5.6.37-82.2</a></td><td><a href="/kb/en/mariadb-1029-release-notes/">MariaDB 10.2.9</a>, <a href="/kb/en/mariadb-10127-release-notes/">MariaDB 10.1.27</a>, <a href="/kb/en/mariadb-10033-release-notes/">MariaDB 10.0.33</a></td><td>Stable</td></tr>
<tr><td>TokuDB from <a href="https://www.percona.com/doc/percona-server/5.6/release-notes/Percona-Server-5.6.36-82.1.html">Percona Server 5.6.36-82.1</a></td><td><a href="/kb/en/mariadb-1028-release-notes/">MariaDB 10.2.8</a>, <a href="/kb/en/mariadb-10126-release-notes/">MariaDB 10.1.26</a>, <a href="/kb/en/mariadb-10032-release-notes/">MariaDB 10.0.32</a></td><td>Stable</td></tr>
<tr><td>TokuDB from <a href="https://www.percona.com/doc/percona-server/5.6/release-notes/Percona-Server-5.6.36-82.0.html">Percona Server 5.6.36-82.0</a></td><td><a href="/kb/en/mariadb-1027-release-notes/">MariaDB 10.2.7</a>, <a href="/kb/en/mariadb-10124-release-notes/">MariaDB 10.1.24</a>, <a href="/kb/en/mariadb-10031-release-notes/">MariaDB 10.0.31</a></td><td>Stable</td></tr>
<tr><td>TokuDB from <a href="https://www.percona.com/doc/percona-server/5.6/release-notes/Percona-Server-5.6.35-80.0.html">Percona Server 5.6.35-80.0</a></td><td><a href="/kb/en/mariadb-1025-release-notes/">MariaDB 10.2.5</a>, <a href="/kb/en/mariadb-10122-release-notes/">MariaDB 10.1.22</a>, <a href="/kb/en/mariadb-10030-release-notes/">MariaDB 10.0.30</a></td><td>Stable</td></tr>
<tr><td>TokuDB from <a href="https://www.percona.com/doc/percona-server/5.6/release-notes/Percona-Server-5.6.34-79.1.html">Percona Server 5.6.34-79.1</a></td><td><a href="/kb/en/mariadb-10120-release-notes/">MariaDB 10.1.20</a>, <a href="/kb/en/mariadb-10029-release-notes/">MariaDB 10.0.29</a></td><td>Stable</td></tr>
<tr><td>TokuDB from <a href="https://www.percona.com/doc/percona-server/5.6/release-notes/Percona-Server-5.6.33-79.0.html">Percona Server 5.6.33-79.0</a></td><td><a href="/kb/en/mariadb-10119-release-notes/">MariaDB 10.1.19</a>, <a href="/kb/en/mariadb-10028-release-notes/">MariaDB 10.0.28</a></td><td>Stable</td></tr>
<tr><td>TokuDB from <a href="https://www.percona.com/doc/percona-server/5.6/release-notes/Percona-Server-5.6.32-78.1.html">Percona Server 5.6.32-78.1</a></td><td><a href="/kb/en/mariadb-10118-release-notes/">MariaDB 10.1.18</a></td><td>Stable</td></tr>
<tr><td>TokuDB from <a href="https://www.percona.com/doc/percona-server/5.6/release-notes/Percona-Server-5.6.31-77.0.html">Percona Server 5.6.31-77.0</a></td><td><a href="/kb/en/mariadb-10117-release-notes/">MariaDB 10.1.17</a>, <a href="/kb/en/mariadb-10027-release-notes/">MariaDB 10.0.27</a></td><td>Stable</td></tr>
<tr><td>TokuDB from <a href="https://www.percona.com/doc/percona-server/5.6/release-notes/Percona-Server-5.6.30-76.3.html">Percona Server 5.6.30-76.3</a></td><td><a href="/kb/en/mariadb-10115-release-notes/">MariaDB 10.1.15</a>, <a href="/kb/en/mariadb-10026-release-notes/">MariaDB 10.0.26</a></td><td>Stable</td></tr>
<tr><td>TokuDB from <a href="https://www.percona.com/doc/percona-server/5.6/release-notes/Percona-Server-5.6.30-76.3.html">Percona Server 5.6.26-74.0</a> <sup class="reference" id="_ref-0">[<a href="#_note-0">1</a>]</sup></td><td><a href="/kb/en/mariadb-10023-release-notes/">MariaDB 10.0.23</a></td><td>Stable</td></tr>
<tr><td><a href="http://docs.tokutek.com/tokudb/tokudb-release-notes.html#tokudb-7-5-7">TokuDB 7.5.7</a></td><td><a href="/kb/en/mariadb-10020-release-notes/">MariaDB 10.0.20</a>, <a href="/kb/en/mariadb-5544-release-notes/">MariaDB 5.5.44</a></td><td>Stable</td></tr>
<tr><td><a href="http://docs.tokutek.com/tokudb/tokudb-release-notes.html#tokudb-7-5-6">TokuDB 7.5.6</a></td><td><a href="/kb/en/mariadb-1015-release-notes/">MariaDB 10.1.5</a>, <a href="/kb/en/mariadb-10018-release-notes/">MariaDB 10.0.18</a>, <a href="/kb/en/mariadb-5543-release-notes/">MariaDB 5.5.43</a></td><td>Stable</td></tr>
<tr><td><a href="http://docs.tokutek.com/tokudb/tokudb-release-notes.html#tokudb-7-5-5">TokuDB 7.5.5</a></td><td><a href="/kb/en/mariadb-1013-release-notes/">MariaDB 10.1.3</a>, <a href="/kb/en/mariadb-10017-release-notes/">MariaDB 10.0.17</a>, <a href="/kb/en/mariadb-5542-release-notes/">MariaDB 5.5.42</a></td><td>Stable</td></tr>
<tr><td><a href="http://docs.tokutek.com/tokudb/tokudb-release-notes.html#tokudb-7-5-4">TokuDB 7.5.4</a></td><td><a href="/kb/en/mariadb-10016-release-notes/">MariaDB 10.0.16</a></td><td>Stable</td></tr>
<tr><td><a href="http://docs.tokutek.com/tokudb/tokudb-release-notes.html#tokudb-7-5-3">TokuDB 7.5.3</a></td><td><a href="/kb/en/mariadb-1012-release-notes/">MariaDB 10.1.2</a>, <a href="/kb/en/mariadb-10015-release-notes/">MariaDB 10.0.15</a>, <a href="/kb/en/mariadb-5541-release-notes/">MariaDB 5.5.41</a></td><td>Stable</td></tr>
<tr><td><a href="http://docs.tokutek.com/tokudb/tokudb-release-notes.html#tokudb-7-5-0">TokuDB 7.5.0</a></td><td><a href="/kb/en/mariadb-10014-release-notes/">MariaDB 10.0.14</a>, <a href="/kb/en/mariadb-5540-release-notes/">MariaDB 5.5.40</a></td><td>Stable</td></tr>
<tr><td><a href="http://docs.tokutek.com/tokudb/tokudb-release-notes.html#tokudb-7-1-7">TokuDB 7.1.7</a></td><td><a href="/kb/en/mariadb-10013-release-notes/">MariaDB 10.0.13</a>, <a href="/kb/en/mariadb-5539-release-notes/">MariaDB 5.5.39</a></td><td>Stable</td></tr>
<tr><td><a href="http://docs.tokutek.com/tokudb/tokudb-release-notes.html#tokudb-7-1-6">TokuDB 7.1.6</a></td><td><a href="/kb/en/mariadb-10011-release-notes/">MariaDB 10.0.11</a>, <a href="/kb/en/mariadb-5538-release-notes/">MariaDB 5.5.38</a></td><td>Stable</td></tr>
<tr><td><a href="http://docs.tokutek.com/tokudb/tokudb-release-notes.html#tokudbv-7-1-5">TokuDB 7.1.5</a></td><td><a href="/kb/en/mariadb-10010-release-notes/">MariaDB 10.0.10</a>, <a href="/kb/en/mariadb-5537-release-notes/">MariaDB 5.5.37</a></td><td>Stable</td></tr>
<tr><td><a href="http://docs.tokutek.com/tokudb/tokudb-release-notes.html#tokudb-7-1-0">TokuDB 7.1.0</a></td><td><a href="/kb/en/mariadb-5534-release-notes/">MariaDB 5.5.34</a></td><td>Stable</td></tr>
<tr><td><a href="http://docs.tokutek.com/tokudb/tokudb-release-notes.html#tokudb-7-0-4">TokuDB 7.0.4</a></td><td><a href="/kb/en/mariadb-1005-release-notes/">MariaDB 10.0.5</a>, <a href="/kb/en/mariadb-5533-release-notes/">MariaDB 5.5.33</a></td><td>Stable</td></tr>
</tbody></table>

The version of TokuDB in your local MariaDB is available by querying the  [tokudb_version](/kb/en/tokudb-system-and-status-variables/#tokudb_version) status variable. For example:

```sql
SHOW VARIABLES LIKE 'tokudb_version';
```

In the MariaDB binary tarballs, only the ones labeled "glibc_214" have TokuDB.

1 [↑](#_ref-0) with this version, TokuDB now follows the version numbering of Percona XtraDB

More information about TokuDB in MariaDB can be found on the following pages:

- [Installing TokuDB](/columns-storage-engines-and-plugins/storage-engines/tokudb/installing-tokudb/) — How to install and enable TokuDB in MariaDB.
- [TokuDB Differences](/columns-storage-engines-and-plugins/storage-engines/tokudb/tokudb-differences/) — Things to know before using TokuDB in MariaDB.
- [TokuDB Resources](/columns-storage-engines-and-plugins/storage-engines/tokudb/tokudb-resources/) — Online TokuDB resources.
- [TokuDB Status Variables](/columns-storage-engines-and-plugins/storage-engines/tokudb/tokudb-status-variables/) — TokuDB status variables
- [TokuDB System Variables](/columns-storage-engines-and-plugins/storage-engines/tokudb/tokudb-system-variables/) — TokuDB System Variables