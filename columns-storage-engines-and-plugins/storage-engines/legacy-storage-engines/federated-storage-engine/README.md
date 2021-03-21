# FEDERATED Storage Engine

The FEDERATED Storage Engine is a legacy storage engine no longer being supported. A fork, [FederatedX](/kb/en/federatedx/) is being actively maintained. Since [MariaDB 10.0](/kb/en/what-is-mariadb-100/), the [CONNECT](/columns-storage-engines-and-plugins/storage-engines/connect) storage engine also permits accessing a remote database via MySQL or ODBC connection (table types: [MYSQL](/kb/en/connect-table-types-mysql-table-type-accessing-mysqlmariadb-tables/), [ODBC](/kb/en/connect-table-types-odbc-table-type-accessing-tables-from-other-dbms/)).

The FEDERATED Storage Engine was originally designed to let one access data remotely without using clustering or replication, and perform local queries that automatically access the remote data.