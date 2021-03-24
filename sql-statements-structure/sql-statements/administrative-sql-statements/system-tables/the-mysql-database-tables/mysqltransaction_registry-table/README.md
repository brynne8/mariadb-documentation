# mysql.transaction_registry Table

##### MariaDB starting with [10.3.4](/kb/en/mariadb-1034-release-notes/)

The `mysql.transaction_registry` table was introduced in [MariaDB 10.3.4](/kb/en/mariadb-1034-release-notes/) as part of [system-versioned tables](/sql-statements-structure/temporal-tables/system-versioned-tables/).

The `mysql.transaction_registry` table is used for transaction-precise versioning, and contains the following fields:

<table><tbody><tr><th>Field</th><th>Type</th><th>Null</th><th>Key</th><th>Default</th><th>Description</th></tr>
<tr><td>transaction_id</td><td>bigint(20) unsigned</td><td>NO</td><td>Primary</td><td>NULL</td><td></td></tr>
<tr><td>commit_id</td><td>bigint(20) unsigned</td><td>NO</td><td>Unique</td><td>NULL</td><td></td></tr>
<tr><td>begin_timestamp</td><td>timestamp(6)</td><td>NO</td><td>Multiple</td><td>0000-00-00 00:00:00.000000</td><td>Timestamp when the transaction began (BEGIN statement), however see <a href="https://jira.mariadb.org/browse/MDEV-16024">MDEV-16024</a>.</td></tr>
<tr><td>commit</td><td>timestamp(6)</td><td>NO</td><td>Multiple</td><td>0000-00-00 00:00:00.000000</td><td>Timestamp when the transaction was committed.</td></tr>
<tr><td>isolation_level</td><td>enum('READ-UNCOMMITTED','READ-COMMITTED','REPEATABLE-READ','SERIALIZABLE')</td><td>NO</td><td></td><td>NULL</td><td>Transaction <a href="/kb/en/set-transaction/#isolation-levels">isolation level</a>.</td></tr>
</tbody></table>