# Spider

The Spider storage engine supports partitioning and [xa transactions](/sql-statements-structure/sql-statements/transactions/xa-transactions/), and allows tables of different MariaDB instances to be handled as if they were on the same instance.

### Versions of Spider in MariaDB

<table><tbody><tr><th>Spider Version</th><th>Introduced</th><th>Maturity</th></tr>
<tr><td>Spider 3.3.15</td><td><a href="/kb/en/mariadb-1054-release-notes/">MariaDB 10.5.4</a></td><td>Gamma</td></tr>
<tr><td>Spider 3.3.14</td><td><a href="/kb/en/mariadb-1043-release-notes/">MariaDB 10.4.3</a>, <a href="/kb/en/mariadb-10313-release-notes/">MariaDB 10.3.13</a></td><td>Stable</td></tr>
<tr><td>Spider 3.3.13</td><td><a href="/kb/en/mariadb-1037-release-notes/">MariaDB 10.3.7</a></td><td>Stable</td></tr>
<tr><td>Spider 3.3.13</td><td><a href="/kb/en/mariadb-1033-release-notes/">MariaDB 10.3.3</a></td><td>Gamma</td></tr>
<tr><td>Spider 3.2.37</td><td><a href="/kb/en/mariadb-10110-release-notes/">MariaDB 10.1.10</a>, <a href="/kb/en/mariadb-10023-release-notes/">MariaDB 10.0.23</a></td><td>Gamma</td></tr>
<tr><td>Spider 3.2.21</td><td><a href="/kb/en/mariadb-1015-release-notes/">MariaDB 10.1.5</a>, <a href="/kb/en/mariadb-10018-release-notes/">MariaDB 10.0.18</a></td><td>Gamma</td></tr>
<tr><td>Spider 3.2.18</td><td><a href="/kb/en/mariadb-10017-release-notes/">MariaDB 10.0.17</a></td><td>Gamma</td></tr>
<tr><td>Spider 3.2.11</td><td><a href="/kb/en/mariadb-10014-release-notes/">MariaDB 10.0.14</a></td><td>Gamma</td></tr>
<tr><td>Spider 3.2.4</td><td><a href="/kb/en/mariadb-10012-release-notes/">MariaDB 10.0.12</a></td><td>Gamma</td></tr>
<tr><td>Spider 3.2</td><td><a href="/kb/en/mariadb-10011-release-notes/">MariaDB 10.0.11</a></td><td>Gamma</td></tr>
<tr><td>Spider 3.0</td><td><a href="/kb/en/mariadb-1004-release-notes/">MariaDB 10.0.4</a></td><td>Beta</td></tr>
</tbody></table>

## Spider Documentation

See the [spider-2.0-doc](http://bazaar.launchpad.net/~kentokushiba/spiderformysql/spider-2.0-doc/files) repository for complete, older, documentation.

[Presentation for new sharding features in Spider 3.3](https://speakerdeck.com/kentoku/new-features-and-enhancements-of-spider-storage-engine-for-sharding).

- [Spider Storage Engine Overview](/columns-storage-engines-and-plugins/storage-engines/spider/spider-storage-engine-overview/) — Storage engine with sharding features.
- [Spider Installation](/columns-storage-engines-and-plugins/storage-engines/spider/spider-installation/) — Setting up Spider.
- [Spider Storage Engine Core Concepts](/columns-storage-engines-and-plugins/storage-engines/spider/spider-storage-engine-core-concepts/) — Key Spider concepts
- [Spider Use Cases](/columns-storage-engines-and-plugins/storage-engines/spider/spider-use-cases/) — Basic working examples for Spider
- [Spider Cluster Management](/columns-storage-engines-and-plugins/storage-engines/spider/spider-cluster-management/) — Spider Cluster Management
- [Spider Feature Matrix](/columns-storage-engines-and-plugins/storage-engines/spider/spider-feature-matrix/) — Matrix of Spider features
- [Spider Server System Variables](/columns-storage-engines-and-plugins/storage-engines/spider/spider-server-system-variables/) — System variables for the Spider storage engine.
- [Spider Table System Variables](/columns-storage-engines-and-plugins/storage-engines/spider/spider-table-system-variables/) — Spider variables available in the CREATE TABLE ... COMMENT clause
- [Spider Status Variables](/replication/optimization-and-tuning/system-variables/spider-status-variables/) — Spider server status variables.
- [Spider Functions](/columns-storage-engines-and-plugins/storage-engines/spider/spider-functions/) — User-defined functions available with the Spider storage engine.
- [Spider Differences Between SpiderForMySQL and MariaDB](/columns-storage-engines-and-plugins/storage-engines/spider/spider-differences-between-spiderformysql-and-mariadb/) — Spider differences between MySQL and MariaDB
- [Information Schema SPIDER_ALLOC_MEM Table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-spider_alloc_mem-table/) — Information about Spider's memory usage.
- [Spider Case Studies](/columns-storage-engines-and-plugins/storage-engines/spider/spider-case-studies/) — List of clients using Spider
- [Spider Benchmarks](/columns-storage-engines-and-plugins/storage-engines/spider/spider-benchmarks/) — Benchmarks for Spider
- [Spider FAQ](/columns-storage-engines-and-plugins/storage-engines/spider/spider-faq/) — Frequently-asked questions about the Spider storage engine
- [Information Schema SPIDER_WRAPPER_PROTOCOLS Table](/columns-storage-engines-and-plugins/storage-engines/spider/information-schema-spider_wrapper_protocols-table/) — MariaDB starting with <a href="/kb/en/mariadb-1054-release-notes/">10.5.4</...