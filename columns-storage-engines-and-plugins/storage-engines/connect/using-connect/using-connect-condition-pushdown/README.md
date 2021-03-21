# Using CONNECT - Condition Pushdown

The [ODBC](/kb/en/connect-table-types-odbc-table-type-accessing-tables-from-other-dbms/), [JDBC](/kb/en/connect-jdbc-table-type-accessing-tables-from-other-dbms/), [MYSQL](/kb/en/connect-table-types-mysql-table-type-accessing-mysqlmariadb-tables/), [TBL](/kb/en/connect-table-types-tbl-table-type-table-list/) and WMI table types use engine condition pushdown in order to restrict the number of rows returned by the RDBS source or the WMI component.

##### MariaDB starting with [10.0.4](/kb/en/mariadb-1004-release-notes/)

Since [MariaDB 10.0.4](/kb/en/mariadb-1004-release-notes/), the CONDITION_PUSHDOWN argument is no longer
accepted. However, it is not needed because CONNECT now uses condition pushdown
unconditionally.

##### MariaDB until [10.0.4](/kb/en/mariadb-1004-release-notes/)

If the engine condition pushdown is OFF, it is necessary to set it ON with the [optimizer_switch](/kb/en/server-system-variables/#optimizer_switch) setting, for instance by:

```sql
set optimizer_switch='engine_condition_pushdown=on';
```

Or launching mysqld with this parameter set to ON, for instance:

```sql
mysqld --console --engine_condition_pushdown=on
```

<strong>Note 1:</strong> specifying <a undefined>--console</a> is important to have some error messages from CONNECT printed because MariaDB does not always retrieve them.