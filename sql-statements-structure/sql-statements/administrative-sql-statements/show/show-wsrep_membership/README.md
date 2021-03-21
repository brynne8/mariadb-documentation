# SHOW WSREP_MEMBERSHIP

##### MariaDB [10.1.2](/kb/en/mariadb-1012-release-notes/)

`SHOW WSREP_MEMBERSHIP` was introduced with the [WSREP_INFO](/columns-storage-engines-and-plugins/plugins/mariadb-replication-cluster-plugins/wsrep_info-plugin) plugin in [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/).

## Syntax

```sql
SHOW WSREP_MEMBERSHIP
```

## Description

The `SHOW WSREP_MEMBERSHIP` statement returns [Galera](/kb/en/galera/) node cluster membership information. It returns the same information as found in the <a undefined>information_schema.WSREP_MEMBERSHIP</a> table. Only users with the [SUPER](/sql-statements-structure/sql-statements/account-management-sql-commands/grant) privilege can access this information.

## Examples

```sql
SHOW WSREP_MEMBERSHIP;
+-------+--------------------------------------+----------+-----------------+
| Index | Uuid                                 | Name     | Address         |
+-------+--------------------------------------+----------+-----------------+
|     0 | 19058073-8940-11e4-8570-16af7bf8fced | my_node1 | 10.0.2.15:16001 |
|     1 | 19f2b0e0-8942-11e4-9cb8-b39e8ee0b5dd | my_node3 | 10.0.2.15:16003 |
|     2 | d85e62db-8941-11e4-b1ef-4bc9980e476d | my_node2 | 10.0.2.15:16002 |
+-------+--------------------------------------+----------+-----------------+
```