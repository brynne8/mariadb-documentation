# InnoDB Versions

##### MariaDB starting with [10.3.7](/kb/en/mariadb-1037-release-notes/)

In [MariaDB 10.3.7](/kb/en/mariadb-1037-release-notes/) and later, the InnoDB implementation has diverged substantially from the InnoDB in MySQL. Therefore, in these versions, the InnoDB version is no longer associated with a MySQL release version.

##### MariaDB starting with [10.2](/kb/en/what-is-mariadb-102/)

In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, the default InnoDB implementation is based on InnoDB from MySQL 5.7. See [Why MariaDB uses InnoDB instead of XtraDB from MariaDB 10.2](/kb/en/why-does-mariadb-102-use-innodb-instead-of-xtradb/) for more information.

##### MariaDB until [10.1](/kb/en/what-is-mariadb-101/)

In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and before, the default InnoDB implementation is based on Percona's XtraDB. XtraDB is a performance enhanced fork of InnoDB. For compatibility reasons, the [system variables](/kb/en/xtradbinnodb-server-system-variables/) still retain their original `innodb` prefixes. If the documentation says that something applies to InnoDB, then it usually also applies to the XtraDB fork, unless explicitly stated otherwise. In these versions, it is still possible to use InnoDB instead of XtraDB. See [Using InnoDB instead of XtraDB](/columns-storage-engines-and-plugins/storage-engines/innodb/using-innodb-instead-of-xtradb) for more information.

##### MariaDB until [10.0.8](/kb/en/mariadb-1008-release-notes/)

In [MariaDB 10.0.8](/kb/en/mariadb-1008-release-notes/) and before, the default InnoDB implementation is based on InnoDB from MySQL. In these versions, Percona's XtraDB is available to use as a dynamic plugin. To use XtraDB in these versions, the <a undefined>ignore_builtin_innodb</a> system variable needs to be set, and the `ha_xtradb` plugin needs to be loaded with the <a undefined>plugin_load</a> option. Both can be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files) prior to starting up the server. See [Installing Plugins](/kb/en/plugin-overview/#installing-plugins) for more information. For example:

```sql
[mariadb]
...
ignore_builtin_innodb
plugin_load=ha_xtradb
```

## Divergences

Some examples of divergences between MariaDB's InnoDB and MySQL's InnoDB are:

- [MariaDB 10.1](/kb/en/what-is-mariadb-101/) (which is based on MySQL 5.6) included encryption and
variable-size page compression before MySQL 5.7 introduced them.

- [MariaDB 10.2](/kb/en/what-is-mariadb-102/) (based on MySQL 5.7) introduced persistent AUTO_INCREMENT ([MDEV-6076](https://jira.mariadb.org/browse/MDEV-6076)) in a GA release before MySQL 8.0.

- [MariaDB 10.3](/kb/en/what-is-mariadb-103/) (based on MySQL 5.7) introduced instant ADD COLUMN ([MDEV-11369](https://jira.mariadb.org/browse/MDEV-11369)) before MySQL.

## InnoDB Versions Included in MariaDB Releases

### [MariaDB 10.3](/kb/en/what-is-mariadb-103/)

<table><tbody><tr><th>InnoDB Version</th><th>Introduced</th></tr>
<tr><td>No longer reported</td><td><a href="/kb/en/mariadb-1037-release-notes/">MariaDB 10.3.7</a></td></tr>
<tr><td>InnoDB 5.7.20</td><td><a href="/kb/en/mariadb-1033-release-notes/">MariaDB 10.3.3</a></td></tr>
<tr><td>InnoDB 5.7.19</td><td><a href="/kb/en/mariadb-1031-release-notes/">MariaDB 10.3.1</a></td></tr>
<tr><td>InnoDB 5.7.14</td><td><a href="/kb/en/mariadb-1030-release-notes/">MariaDB 10.3.0</a></td></tr>
</tbody></table>

### [MariaDB 10.2](/kb/en/what-is-mariadb-102/)

<table><tbody><tr><th>InnoDB Version</th><th>Introduced</th></tr>
<tr><td>InnoDB 5.7.29</td><td><a href="/kb/en/mariadb-10233-release-notes/">MariaDB 10.2.33</a></td></tr>
<tr><td>InnoDB 5.7.23</td><td><a href="/kb/en/mariadb-10217-release-notes/">MariaDB 10.2.17</a></td></tr>
<tr><td>InnoDB 5.7.22</td><td><a href="/kb/en/mariadb-10215-release-notes/">MariaDB 10.2.15</a></td></tr>
<tr><td>InnoDB 5.7.21</td><td><a href="/kb/en/mariadb-10213-release-notes/">MariaDB 10.2.13</a></td></tr>
<tr><td>InnoDB 5.7.20</td><td><a href="/kb/en/mariadb-10210-release-notes/">MariaDB 10.2.10</a></td></tr>
<tr><td>InnoDB 5.7.19</td><td><a href="/kb/en/mariadb-1028-release-notes/">MariaDB 10.2.8</a></td></tr>
<tr><td>InnoDB 5.7.18</td><td><a href="/kb/en/mariadb-1027-release-notes/">MariaDB 10.2.7</a></td></tr>
<tr><td>InnoDB 5.7.14</td><td><a href="/kb/en/mariadb-1022-release-notes/">MariaDB 10.2.2</a></td></tr>
</tbody></table>

### [MariaDB 10.1](/kb/en/what-is-mariadb-101/)

<table><tbody><tr><th>InnoDB Version</th><th>Introduced</th></tr>
<tr><td>InnoDB 5.6.49</td><td><a href="/kb/en/mariadb-10146-release-notes/">MariaDB 10.1.46</a></td></tr>
<tr><td>InnoDB 5.6.47</td><td><a href="/kb/en/mariadb-10144-release-notes/">MariaDB 10.1.44</a></td></tr>
<tr><td>InnoDB 5.6.44</td><td><a href="/kb/en/mariadb-10139-release-notes/">MariaDB 10.1.39</a></td></tr>
<tr><td>InnoDB 5.6.42</td><td><a href="/kb/en/mariadb-10137-release-notes/">MariaDB 10.1.37</a></td></tr>
<tr><td>InnoDB 5.6.39</td><td><a href="/kb/en/mariadb-10131-release-notes/">MariaDB 10.1.31</a></td></tr>
<tr><td>InnoDB 5.6.37</td><td><a href="/kb/en/mariadb-10126-release-notes/">MariaDB 10.1.26</a></td></tr>
<tr><td>InnoDB 5.6.36</td><td><a href="/kb/en/mariadb-10124-release-notes/">MariaDB 10.1.24</a></td></tr>
<tr><td>InnoDB 5.6.35</td><td><a href="/kb/en/mariadb-10121-release-notes/">MariaDB 10.1.21</a></td></tr>
<tr><td>InnoDB 5.6.33</td><td><a href="/kb/en/mariadb-10118-release-notes/">MariaDB 10.1.18</a></td></tr>
<tr><td>InnoDB 5.6.32</td><td><a href="/kb/en/mariadb-10117-release-notes/">MariaDB 10.1.17</a></td></tr>
<tr><td>InnoDB 5.6.31</td><td><a href="/kb/en/mariadb-10116-release-notes/">MariaDB 10.1.16</a></td></tr>
<tr><td>InnoDB 5.6.30</td><td><a href="/kb/en/mariadb-10114-release-notes/">MariaDB 10.1.14</a></td></tr>
<tr><td>InnoDB 5.6.29</td><td><a href="/kb/en/mariadb-10112-release-notes/">MariaDB 10.1.12</a></td></tr>
</tbody></table>

### [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

<table><tbody><tr><th>InnoDB Version</th><th>Introduced</th></tr>
<tr><td>InnoDB 5.6.43</td><td><a href="/kb/en/mariadb-10038-release-notes/">MariaDB 10.0.38</a></td></tr>
<tr><td>InnoDB 5.6.42</td><td><a href="/kb/en/mariadb-10037-release-notes/">MariaDB 10.0.37</a></td></tr>
<tr><td>InnoDB 5.6.40</td><td><a href="/kb/en/mariadb-10035-release-notes/">MariaDB 10.0.35</a></td></tr>
<tr><td>InnoDB 5.6.39</td><td><a href="/kb/en/mariadb-10034-release-notes/">MariaDB 10.0.34</a></td></tr>
<tr><td>InnoDB 5.6.38</td><td><a href="/kb/en/mariadb-10033-release-notes/">MariaDB 10.0.33</a></td></tr>
<tr><td>InnoDB 5.6.37</td><td><a href="/kb/en/mariadb-10032-release-notes/">MariaDB 10.0.32</a></td></tr>
<tr><td>InnoDB 5.6.36</td><td><a href="/kb/en/mariadb-10031-release-notes/">MariaDB 10.0.31</a></td></tr>
<tr><td>InnoDB 5.6.35</td><td><a href="/kb/en/mariadb-10029-release-notes/">MariaDB 10.0.29</a></td></tr>
<tr><td>InnoDB 5.6.33</td><td><a href="/kb/en/mariadb-10028-release-notes/">MariaDB 10.0.28</a></td></tr>
<tr><td>InnoDB 5.6.32</td><td><a href="/kb/en/mariadb-10027-release-notes/">MariaDB 10.0.27</a></td></tr>
<tr><td>InnoDB 5.6.31</td><td><a href="/kb/en/mariadb-10026-release-notes/">MariaDB 10.0.26</a></td></tr>
<tr><td>InnoDB 5.6.30</td><td><a href="/kb/en/mariadb-10025-release-notes/">MariaDB 10.0.25</a></td></tr>
<tr><td>InnoDB 5.6.29</td><td><a href="/kb/en/mariadb-10024-release-notes/">MariaDB 10.0.24</a></td></tr>
<tr><td>InnoDB 5.6.28</td><td><a href="/kb/en/mariadb-10023-release-notes/">MariaDB 10.0.23</a></td></tr>
<tr><td>InnoDB 5.6.27</td><td><a href="/kb/en/mariadb-10022-release-notes/">MariaDB 10.0.22</a></td></tr>
<tr><td>InnoDB 5.6.26</td><td><a href="/kb/en/mariadb-10021-release-notes/">MariaDB 10.0.21</a></td></tr>
<tr><td>InnoDB 5.6.25</td><td><a href="/kb/en/mariadb-10020-release-notes/">MariaDB 10.0.20</a></td></tr>
<tr><td>InnoDB 5.6.24</td><td><a href="/kb/en/mariadb-10018-release-notes/">MariaDB 10.0.18</a></td></tr>
<tr><td>InnoDB 5.6.23</td><td><a href="/kb/en/mariadb-10017-release-notes/">MariaDB 10.0.17</a></td></tr>
<tr><td>InnoDB 5.6.22</td><td><a href="/kb/en/mariadb-10016-release-notes/">MariaDB 10.0.16</a></td></tr>
<tr><td>InnoDB 5.6.21</td><td><a href="/kb/en/mariadb-10015-release-notes/">MariaDB 10.0.15</a></td></tr>
<tr><td>InnoDB 5.6.20</td><td><a href="/kb/en/mariadb-10014-release-notes/">MariaDB 10.0.14</a></td></tr>
<tr><td>InnoDB 5.6.19</td><td><a href="/kb/en/mariadb-10013-release-notes/">MariaDB 10.0.13</a></td></tr>
<tr><td>InnoDB 5.6.17</td><td><a href="/kb/en/mariadb-10011-release-notes/">MariaDB 10.0.11</a></td></tr>
<tr><td>InnoDB 5.6.15</td><td><a href="/kb/en/mariadb-1009-release-notes/">MariaDB 10.0.9</a></td></tr>
<tr><td>InnoDB 5.6.14</td><td><a href="/kb/en/mariadb-1008-release-notes/">MariaDB 10.0.8</a></td></tr>
</tbody></table>

## See Also

- [Why MariaDB uses InnoDB instead of XtraDB from MariaDB 10.2](/kb/en/why-does-mariadb-102-use-innodb-instead-of-xtradb/)
- [XtraDB Versions](/columns-storage-engines-and-plugins/storage-engines/innodb/about-xtradb)