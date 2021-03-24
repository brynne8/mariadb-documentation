# Information Schema INNODB_LOCK_WAITS Table

The [Information Schema](/kb/en/information_schema/) `INNODB_LOCK_WAITS` table contains information about blocked InnoDB transactions. The `PROCESS` [privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) is required to view the table.

It contains the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>REQUESTING_TRX_ID</code></td><td>Requesting transaction ID from the <a href="/kb/en/information-schema-innodb_trx-table/">INNODB_TRX</a> table.</td></tr>
<tr><td><code>REQUESTED_LOCK_ID</code></td><td>Lock ID from the <a href="/kb/en/information-schema-innodb_locks-table/">INNODB.LOCKS</a> table for the waiting transaction.</td></tr>
<tr><td><code>BLOCKING_TRX_ID</code></td><td>Blocking transaction ID from the <a href="/kb/en/information-schema-innodb_trx-table/">INNODB_TRX</a> table.</td></tr>
<tr><td><code>BLOCKING_LOCK_ID</code></td><td>Lock ID from the <a href="/kb/en/information-schema-innodb_locks-table/">INNODB.LOCKS</a> table of a lock held by a transaction that is blocking another transaction.</td></tr>
</tbody></table>

The table is often used in conjunction with the [INNODB_LOCKS](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-innodb-tables/information-schema-innodb_locks-table/) and [INNODB_TRX](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-innodb-tables/information-schema-innodb_trx-table/) tables to diagnose problematic locks and transactions.