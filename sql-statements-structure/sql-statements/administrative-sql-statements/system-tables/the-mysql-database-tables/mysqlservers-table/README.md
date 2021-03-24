# mysql.servers Table

The `mysql.servers` table contains information about servers as used by the [Spider](/columns-storage-engines-and-plugins/storage-engines/spider/), [FEDERATED](/columns-storage-engines-and-plugins/storage-engines/legacy-storage-engines/federated-storage-engine/) or [FederatedX](/kb/en/federatedx/) storage engines (see [CREATE SERVER](/sql-statements-structure/sql-statements/data-definition/create/create-server/)).

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, this table uses the [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/) storage engine.

##### MariaDB until [10.3](/kb/en/what-is-mariadb-103/)

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and before, this table uses the [MyISAM](/columns-storage-engines-and-plugins/storage-engines/myisam-storage-engine/) storage engine.

The `mysql.servers` table contains the following fields:

<table><tbody><tr><th>Field</th><th>Type</th><th>Null</th><th>Key</th><th>Default</th><th>Description</th></tr>
<tr><td><code>Server_name</code></td><td><code>char(64)</code></td><td>NO</td><td>PRI</td><td></td><td></td></tr>
<tr><td><code>Host</code></td><td><code>char(64)</code></td><td>NO</td><td></td><td></td><td></td></tr>
<tr><td><code>Db</code></td><td><code>char(64)</code></td><td>NO</td><td></td><td></td><td></td></tr>
<tr><td><code>Username</code></td><td><code>char(80)</code></td><td>NO</td><td></td><td></td><td></td></tr>
<tr><td><code>Password</code></td><td><code>char(64)</code></td><td>NO</td><td></td><td></td><td></td></tr>
<tr><td><code>Port</code></td><td><code>int(4)</code></td><td>NO</td><td></td><td>0</td><td></td></tr>
<tr><td><code>Socket</code></td><td><code>char(64)</code></td><td>NO</td><td></td><td></td><td></td></tr>
<tr><td><code>Wrapper</code></td><td><code>char(64)</code></td><td>NO</td><td></td><td></td><td><code>mysql</code> or <code>mariadb</code></td></tr>
<tr><td><code>Owner</code></td><td><code>char(64)</code></td><td>NO</td><td></td><td></td><td></td></tr>
</tbody></table>