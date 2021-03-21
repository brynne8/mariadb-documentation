# About Mroonga

<table><tbody><tr><th>Mroonga Version</th><th>Introduced</th><th>Maturity</th></tr>
<tr><td>7.07</td><td><a href="/kb/en/mariadb-10211-release-notes/">MariaDB 10.2.11</a>, <a href="/kb/en/mariadb-10129-release-notes/">MariaDB 10.1.29</a></td><td>Stable</td></tr>
<tr><td>5.04</td><td><a href="/kb/en/mariadb-1016-release-notes/">MariaDB 10.1.6</a></td><td>Stable</td></tr>
<tr><td>5.02</td><td><a href="/kb/en/mariadb-10018-release-notes/">MariaDB 10.0.18</a>, <a href="/kb/en/mariadb-1015-release-notes/">MariaDB 10.1.5</a></td><td>Stable</td></tr>
<tr><td>5.0</td><td><a href="/kb/en/mariadb-10017-release-notes/">MariaDB 10.0.17</a></td><td>Stable</td></tr>
<tr><td>4.06</td><td><a href="/kb/en/mariadb-10015-release-notes/">MariaDB 10.0.15</a></td><td>Stable</td></tr>
</tbody></table>

Mroonga, formerly named Groonga storage engine, was included in the [MariaDB 5.3](/kb/en/what-is-mariadb-53/) release, and is available by default from [MariaDB 10.0.15](/kb/en/mariadb-10015-release-notes/).

Mroonga is a full text search storage engine based on Groonga, which is an open-source CJK-ready fulltext search engine using column base. See [http://groonga.org](http://groonga.org) for more.

With Mroonga, you can have a CJK-ready full text search feature, and it is faster than the [MyISAM](/kb/en/myisam/) and [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) [full text search](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes/) for both updating and searching.

Mroonga also supports Groonga's fast geolocation search by using MariaDB's geolocation SQL syntax.

Mroonga currently only supports Linux x86_64 (Intel64/AMD64).

## How to Install

Before [MariaDB 10.0.15](/kb/en/mariadb-10015-release-notes/), Mroonga was not available by default, and you needed to build the plugin. See the instructions at [http://mroonga.org/docs/install.html](http://mroonga.org/docs/install.html). Since [MariaDB 10.0.15](/kb/en/mariadb-10015-release-notes/), or once the plugin has been built, enable Mroonga with the following statement:

```sql
mysql> INSTALL SONAME 'ha_mroonga';
```

On Debian and Ubuntu mroonga engine will be installed with

```sql
sudo apt-get install mariadb-plugin-mroonga
```

See [Plugin overview](/columns-storage-engines-and-plugins/plugins/plugin-overview/) for details on installing and uninstalling plugins.

[SHOW ENGINES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engines/) can be used to check whether Mroonga is installed correctly:

```sql
SHOW ENGINES;
...
*************************** 8. row ***************************
      Engine: Mroonga
     Support: YES
     Comment: CJK-ready fulltext search, column store
Transactions: NO
          XA: NO
  Savepoints: NO
...
```

Once the plugin is installed, add a UDF (User-Defined Function) named "last_insert_grn_id", that returns the record ID assigned by groonga in INSERT, by the following SQL.

```sql
mysql> CREATE FUNCTION last_insert_grn_id RETURNS INTEGER SONAME 'ha_mroonga.so';
```

## Limitations

- The maximum size of a single key is 4096 bytes.
- The maximum size of all keys is 4GB.
- The maximum number of records in a fulltext index is 268,435,455
- The maximum number of distinct terms in a fulltext index is 268,435,455
- The maximum size of a fulltext index is 256GB

Note that the maximum sizes are not hard limits, and may vary according to circumstance.

For more details, see [http://mroonga.org/docs/reference/limitations.html](http://mroonga.org/docs/reference/limitations.html).

## Available Character Sets

Mroonga supports a limited number of [character sets](/kb/en/data-types-character-sets-and-collations/). These include:

- ASCII
- BINARY
- CP932
- EUCJPMS
- KOI8R
- LATIN1
- SJIS
- UJIS
- UTF8
- UTF8MB4

## More Information

Further documentation for Mroonga can be found at [http://mroonga.org/docs/](http://mroonga.org/docs/)