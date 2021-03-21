# SHOW WSREP_STATUS

##### MariaDB [10.1.2](/kb/en/mariadb-1012-release-notes/)

`SHOW WSREP_STATUS` was introduced with the [WSREP_INFO](/columns-storage-engines-and-plugins/plugins/mariadb-replication-cluster-plugins/wsrep_info-plugin) plugin in [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/).

## Syntax

```sql
SHOW WSREP_STATUS
```

## Description

The `SHOW WSREP_STATUS` statement returns [Galera](/kb/en/galera/) node and cluster status information. It returns the same information as found in the <a undefined>information_schema.WSREP_STATUS</a> table. Only users with the [SUPER](/sql-statements-structure/sql-statements/account-management-sql-commands/grant) privilege can access this information.

## Examples

```sql
SHOW WSREP_STATUS;
+------------+-------------+----------------+--------------+
| Node_Index | Node_Status | Cluster_Status | Cluster_Size |
+------------+-------------+----------------+--------------+
|          0 | Synced      | Primary        |            3 |
+------------+-------------+----------------+--------------+
```