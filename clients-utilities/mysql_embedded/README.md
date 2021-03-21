# mysql_embedded

##### MariaDB starting with [10.4.6](/kb/en/mariadb-1046-release-notes/)

From [MariaDB 10.4.6](/kb/en/mariadb-1046-release-notes/), `mariadb-embedded` is a symlink to `mysql_embedded`.

##### MariaDB starting with [10.5.2](/kb/en/mariadb-1052-release-notes/)

From [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), `mariadb-embedded` is the name of the tool, with `mysql_embedded` a symlink.

`mysql_embedded` is a [mysql client](/clients-utilities/mysql-client/mysql-command-line-client/) statically linked to libmysqld, the embedded server. Upon execution, an embedded MariaDB server is instantiated and you can execute statements just as you would using the normal mysql client, using the same options.

Do not run <em>mysql_embedded</em> while MariaDB is running, as effectively it starts a new instance of the server.

## Examples

```sql
sudo mysql_embedded -e 'select user, host, password from mysql.user where user="root"'
+------+-----------+-------------------------------------------+
| user | host      | password                                  |
+------+-----------+-------------------------------------------+
| root | localhost | *196BDEDE2AE4F84CA44C47D54D78478C7E2BD7B7 |
| root | db1       | *196BDEDE2AE4F84CA44C47D54D78478C7E2BD7B7 |
| root | 127.0.0.1 | *196BDEDE2AE4F84CA44C47D54D78478C7E2BD7B7 |
| root | ::1       | *196BDEDE2AE4F84CA44C47D54D78478C7E2BD7B7 |
+------+-----------+-------------------------------------------+
```

Sending options with `--server-arg`:

```sql
sudo mysql_embedded --server-arg='--skip-innodb'
  --server-arg='--default-storage-engine=myisam' 
  --server-arg='--log-error=/tmp/mysql.err' 
  -e 'select user, host, password from mysql.user where user="root"'
+------+-----------+-------------------------------------------+
| user | host      | password                                  |
+------+-----------+-------------------------------------------+
| root | localhost | *196BDEDE2AE4F84CA44C47D54D78478C7E2BD7B7 |
| root | db1       | *196BDEDE2AE4F84CA44C47D54D78478C7E2BD7B7 |
| root | 127.0.0.1 | *196BDEDE2AE4F84CA44C47D54D78478C7E2BD7B7 |
| root | ::1       | *196BDEDE2AE4F84CA44C47D54D78478C7E2BD7B7 |
+------+-----------+-------------------------------------------+
```

## See Also

- [Using mysql_embedded and mysqld --bootstrap to tinker with privilege tables](Using_mysql_embedded_and_mysqld_--bootstrap_to_tinker_with_privilege_tables)