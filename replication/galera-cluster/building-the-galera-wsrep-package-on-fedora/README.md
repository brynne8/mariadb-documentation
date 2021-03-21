# Building the Galera wsrep Package on Fedora

The instructions on this page were used to create the <em>galera</em> package on the Fedora Linux distribution. This package contains the wsrep provider for [MariaDB Galera Cluster](/kb/en/galera/).
<br><br>

The following table lists each version of the [Galera](/kb/en/galera/) 4 wsrep provider, and it lists which version of MariaDB each one was first released in. If you would like to install [Galera](/kb/en/galera/) 4 using [yum](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm/yum), [apt](/kb/en/installing-mariadb-deb-files/#installing-mariadb-with-apt), or [zypper](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm/installing-mariadb-with-zypper), then the package is called `galera-4`.

<table><tbody><tr><th>Galera Version</th><th>Released in MariaDB Version</th></tr>
<tr><td><strong>26.4.7</strong></td><td><a href="/kb/en/mariadb-1059-release-notes/">MariaDB 10.5.9</a>, <a href="/kb/en/mariadb-10418-release-notes/">MariaDB 10.4.18</a></td></tr>
<tr><td><strong>26.4.6</strong></td><td><a href="/kb/en/mariadb-1057-release-notes/">MariaDB 10.5.7</a>, <a href="/kb/en/mariadb-10416-release-notes/">MariaDB 10.4.16</a></td></tr>
<tr><td><strong>26.4.5</strong></td><td><a href="/kb/en/mariadb-1054-release-notes/">MariaDB 10.5.4</a>, <a href="/kb/en/mariadb-10414-release-notes/">MariaDB 10.4.14</a></td></tr>
<tr><td><strong>26.4.4</strong></td><td><a href="/kb/en/mariadb-1051-release-notes/">MariaDB 10.5.1</a>, <a href="/kb/en/mariadb-10413-release-notes/">MariaDB 10.4.13</a></td></tr>
<tr><td><strong>26.4.3</strong></td><td><a href="/kb/en/mariadb-1050-release-notes/">MariaDB 10.5.0</a>, <a href="/kb/en/mariadb-1049-release-notes/">MariaDB 10.4.9</a></td></tr>
<tr><td><strong>26.4.2</strong></td><td><a href="/kb/en/mariadb-1044-release-notes/">MariaDB 10.4.4</a></td></tr>
<tr><td><strong>26.4.1</strong></td><td><a href="/kb/en/mariadb-1043-release-notes/">MariaDB 10.4.3</a></td></tr>
<tr><td><strong>26.4.0</strong></td><td><a href="/kb/en/mariadb-1042-release-notes/">MariaDB 10.4.2</a></td></tr>
</tbody></table>

The following table lists each version of the [Galera](/kb/en/galera/) 3 wsrep provider, and it lists which version of MariaDB each one was first released in. If you would like to install [Galera](/kb/en/galera/) 3 using [yum](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm/yum), [apt](/kb/en/installing-mariadb-deb-files/#installing-mariadb-with-apt), or [zypper](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm/installing-mariadb-with-zypper), then the package is called `galera`.

<table><tbody><tr><th>Galera Version</th><th>Released in MariaDB Version</th></tr>
<tr><td><strong>25.3.32</strong></td><td><a href="/kb/en/mariadb-10328-release-notes/">MariaDB 10.3.28</a>, <a href="/kb/en/mariadb-10237-release-notes/">MariaDB 10.2.37</a></td></tr>
<tr><td><strong>25.3.31</strong></td><td><a href="/kb/en/mariadb-10326-release-notes/">MariaDB 10.3.26</a>, <a href="/kb/en/mariadb-10235-release-notes/">MariaDB 10.2.35</a>, <a href="/kb/en/mariadb-10148-release-notes/">MariaDB 10.1.48</a></td></tr>
<tr><td><strong>25.3.30</strong></td><td><a href="/kb/en/mariadb-10325-release-notes/">MariaDB 10.3.25</a>, <a href="/kb/en/mariadb-10234-release-notes/">MariaDB 10.2.34</a>, <a href="/kb/en/mariadb-10147-release-notes/">MariaDB 10.1.47</a></td></tr>
<tr><td><strong>25.3.29</strong></td><td><a href="/kb/en/mariadb-10323-release-notes/">MariaDB 10.3.23</a>, <a href="/kb/en/mariadb-10232-release-notes/">MariaDB 10.2.32</a>, <a href="/kb/en/mariadb-10145-release-notes/">MariaDB 10.1.45</a></td></tr>
<tr><td><strong>25.3.28</strong></td><td><a href="/kb/en/mariadb-10319-release-notes/">MariaDB 10.3.19</a>, <a href="/kb/en/mariadb-10228-release-notes/">MariaDB 10.2.28</a>, <a href="/kb/en/mariadb-10142-release-notes/">MariaDB 10.1.42</a></td></tr>
<tr><td><strong>25.3.27</strong></td><td><a href="/kb/en/mariadb-10318-release-notes/">MariaDB 10.3.18</a>, <a href="/kb/en/mariadb-10227-release-notes/">MariaDB 10.2.27</a></td></tr>
<tr><td><strong>25.3.26</strong></td><td><a href="/kb/en/mariadb-10314-release-notes/">MariaDB 10.3.14</a>, <a href="/kb/en/mariadb-10223-release-notes/">MariaDB 10.2.23</a>, <a href="/kb/en/mariadb-10139-release-notes/">MariaDB 10.1.39</a></td></tr>
<tr><td><strong>25.3.25</strong></td><td><a href="/kb/en/mariadb-10312-release-notes/">MariaDB 10.3.12</a>, <a href="/kb/en/mariadb-10220-release-notes/">MariaDB 10.2.20</a>, <a href="/kb/en/mariadb-10138-release-notes/">MariaDB 10.1.38</a>, <a href="/kb/en/mariadb-galera-cluster-10038-release-notes/">MariaDB Galera Cluster 10.0.38</a>, <a href="/kb/en/mariadb-galera-cluster-5563-release-notes/">MariaDB Galera Cluster 5.5.63</a></td></tr>
<tr><td><strong>25.3.24</strong></td><td><a href="/kb/en/mariadb-1040-release-notes/">MariaDB 10.4.0</a>, <a href="/kb/en/mariadb-10310-release-notes/">MariaDB 10.3.10</a>, <a href="/kb/en/mariadb-10218-release-notes/">MariaDB 10.2.18</a>, <a href="/kb/en/mariadb-10137-release-notes/">MariaDB 10.1.37</a>, <a href="/kb/en/mariadb-galera-cluster-10037-release-notes/">MariaDB Galera Cluster 10.0.37</a>, <a href="/kb/en/mariadb-galera-cluster-5562-release-notes/">MariaDB Galera Cluster 5.5.62</a></td></tr>
<tr><td><strong>25.3.23</strong></td><td><a href="/kb/en/mariadb-1035-release-notes/">MariaDB 10.3.5</a>, <a href="/kb/en/mariadb-10213-release-notes/">MariaDB 10.2.13</a>, <a href="/kb/en/mariadb-10132-release-notes/">MariaDB 10.1.32</a>, <a href="/kb/en/mariadb-galera-cluster-10035-release-notes/">MariaDB Galera Cluster 10.0.35</a>, <a href="/kb/en/mariadb-galera-cluster-5560-release-notes/">MariaDB Galera Cluster 5.5.60</a></td></tr>
<tr><td><strong>25.3.22</strong></td><td><a href="/kb/en/mariadb-1033-release-notes/">MariaDB 10.3.3</a>, <a href="/kb/en/mariadb-10211-release-notes/">MariaDB 10.2.11</a>, <a href="/kb/en/mariadb-10129-release-notes/">MariaDB 10.1.29</a>, <a href="/kb/en/mariadb-galera-cluster-10033-release-notes/">MariaDB Galera Cluster 10.0.33</a>, <a href="/kb/en/mariadb-galera-cluster-5559-release-notes/">MariaDB Galera Cluster 5.5.59</a></td></tr>
<tr><td><strong>25.3.21</strong></td><td>N/A</td></tr>
<tr><td><strong>25.3.20</strong></td><td><a href="/kb/en/mariadb-1031-release-notes/">MariaDB 10.3.1</a>, <a href="/kb/en/mariadb-1026-release-notes/">MariaDB 10.2.6</a>, <a href="/kb/en/mariadb-10123-release-notes/">MariaDB 10.1.23</a>, <a href="/kb/en/mariadb-galera-cluster-10031-release-notes/">MariaDB Galera Cluster 10.0.31</a>, <a href="/kb/en/mariadb-galera-cluster-5556-release-notes/">MariaDB Galera Cluster 5.5.56</a></td></tr>
<tr><td><strong>25.3.19</strong></td><td><a href="/kb/en/mariadb-1030-release-notes/">MariaDB 10.3.0</a>, <a href="/kb/en/mariadb-1023-release-notes/">MariaDB 10.2.3</a>, <a href="/kb/en/mariadb-10120-release-notes/">MariaDB 10.1.20</a>, <a href="/kb/en/mariadb-galera-cluster-10029-release-notes/">MariaDB Galera Cluster 10.0.29</a>, <a href="/kb/en/mariadb-galera-cluster-5554-release-notes/">MariaDB Galera Cluster 5.5.54</a></td></tr>
<tr><td><strong>25.3.18</strong></td><td><a href="/kb/en/mariadb-1022-release-notes/">MariaDB 10.2.2</a>, <a href="/kb/en/mariadb-10118-release-notes/">MariaDB 10.1.18</a>, <a href="/kb/en/mariadb-galera-cluster-10028-release-notes/">MariaDB Galera Cluster 10.0.28</a>, <a href="/kb/en/mariadb-galera-cluster-5553-release-notes/">MariaDB Galera Cluster 5.5.53</a></td></tr>
<tr><td><strong>25.3.17</strong></td><td><a href="/kb/en/mariadb-10117-release-notes/">MariaDB 10.1.17</a>, <a href="/kb/en/mariadb-galera-cluster-10027-release-notes/">MariaDB Galera Cluster 10.0.27</a>, <a href="/kb/en/mariadb-galera-cluster-5551-release-notes/">MariaDB Galera Cluster 5.5.51</a></td></tr>
<tr><td><strong>25.3.16</strong></td><td>N/A</td></tr>
<tr><td><strong>25.3.15</strong></td><td><a href="/kb/en/mariadb-1020-release-notes/">MariaDB 10.2.0</a>, <a href="/kb/en/mariadb-10113-release-notes/">MariaDB 10.1.13</a>, <a href="/kb/en/mariadb-galera-cluster-10025-release-notes/">MariaDB Galera Cluster 10.0.25</a>, <a href="/kb/en/mariadb-galera-cluster-5549-release-notes/">MariaDB Galera Cluster 5.5.49</a></td></tr>
<tr><td><strong>25.3.14</strong></td><td><a href="/kb/en/mariadb-10112-release-notes/">MariaDB 10.1.12</a>, <a href="/kb/en/mariadb-galera-cluster-10024-release-notes/">MariaDB Galera Cluster 10.0.24</a>, <a href="/kb/en/mariadb-galera-cluster-5548-release-notes/">MariaDB Galera Cluster 5.5.48</a></td></tr>
<tr><td><strong>25.3.12</strong></td><td><a href="/kb/en/mariadb-10111-release-notes/">MariaDB 10.1.11</a></td></tr>
<tr><td><strong>25.3.11</strong></td><td>N/A</td></tr>
<tr><td><strong>25.3.10</strong></td><td>N/A</td></tr>
<tr><td><strong>25.3.9</strong></td><td><a href="/kb/en/mariadb-1013-release-notes/">MariaDB 10.1.3</a>, <a href="/kb/en/mariadb-galera-cluster-10017-release-notes/">MariaDB Galera Cluster 10.0.17</a>, <a href="/kb/en/mariadb-galera-cluster-5542-release-notes/">MariaDB Galera Cluster 5.5.42</a></td></tr>
<tr><td><strong>25.3.8</strong></td><td>N/A</td></tr>
<tr><td><strong>25.3.7</strong></td><td>N/A</td></tr>
<tr><td><strong>25.3.6</strong></td><td>N/A</td></tr>
<tr><td><strong>25.3.5</strong></td><td><a href="/kb/en/mariadb-1011-release-notes/">MariaDB 10.1.1</a>, <a href="/kb/en/mariadb-galera-cluster-10010-release-notes/">MariaDB Galera Cluster 10.0.10</a>, <a href="/kb/en/mariadb-galera-cluster-5537-release-notes/">MariaDB Galera Cluster 5.5.37</a></td></tr>
<tr><td><strong>25.3.4</strong></td><td>N/A</td></tr>
<tr><td><strong>25.3.3</strong></td><td>N/A</td></tr>
<tr><td><strong>25.3.2</strong></td><td><a href="/kb/en/mariadb-galera-cluster-1007-release-notes/">MariaDB Galera Cluster 10.0.7</a>,   <a href="/kb/en/mariadb-galera-cluster-5535-release-notes/">MariaDB Galera Cluster 5.5.35</a></td></tr>
</tbody></table>

The following table lists each version of the [Galera](/kb/en/galera/) 2 wsrep provider, and it lists which version of MariaDB each one was first released in.

<table><tbody><tr><th>Galera Version</th><th>Released in MariaDB Galera Cluster Version</th></tr>
<tr><td><strong>25.2.9</strong></td><td><a href="/kb/en/mariadb-galera-cluster-10010-release-notes/">10.0.10</a>, <a href="/kb/en/mariadb-galera-cluster-5537-release-notes/">5.5.37</a></td></tr>
<tr><td><strong>25.2.8</strong></td><td><a href="/kb/en/mariadb-galera-cluster-1007-release-notes/">10.0.7</a>,   <a href="/kb/en/mariadb-galera-cluster-5535-release-notes/">5.5.35</a></td></tr>
<tr><td><strong>23.2.7</strong></td><td><a href="/kb/en/mariadb-galera-cluster-5534-release-notes/">5.5.34</a></td></tr>
</tbody></table>

For convenience, a <em>galera</em> package containing the <strong>preferred</strong> wsrep provider
is included in the MariaDB
[YUM and APT repositories](https://downloads.mariadb.org/mariadb/repositories/)
(the preferred versions are <strong>bolded</strong> in the table above).

See also [Deciphering Galera Version Numbers](https://mariadb.com/blog/deciphering-galera-version-numbers).

1 Install the prerequisites:

```sql
sudo yum update
sudo yum -y install boost-devel check-devel glibc-devel openssl-devel scons
```

1 Clone [galera.git](https://github.com/mariadb/galera) from [github.com/mariadb](https://github.com/mariadb) and checkout mariadb-3.x banch:

```sql
git init repo
cd repo
git clone -b mariadb-3.x https://github.com/MariaDB/galera.git
```

1 Build the packages by executing `build.sh` under scripts/ directory with `-p` switch:

```sql
cd galera
./scripts/build.sh -p
```

When finished, you will have an RPM package containing galera library, arbitrator and related files in the current directory. Note: The same set of instructions can be applied to other RPM based platforms to generate galera package.