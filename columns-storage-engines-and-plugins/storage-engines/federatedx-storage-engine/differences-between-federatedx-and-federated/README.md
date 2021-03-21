# Differences Between FederatedX and Federated

The main differences are:

## New features in FederatedX

- Transactions (beta feature)
- Supports partitions (alpha feature)
- New class structure which allows developers to write connection classes for other RDBMSs without having to modify base classes for FederatedX
- Actively developed!

## Different behavior

- FederatedX is statically compiled into MariaDB by default.
- When you create a table with FederatedX, the connection will be tested. The `CREATE` will fail if MariaDB can't connect to the remote host or if the remote table doesn't exist.