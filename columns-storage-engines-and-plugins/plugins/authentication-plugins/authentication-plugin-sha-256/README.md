# Authentication Plugin - SHA-256

MySQL 5.6 added support for the <a undefined>sha256_password</a> authentication plugin, and MySQL 8.0 also added support for the <a undefined>caching_sha2_password</a> authentication plugin.

The `caching_sha2_password` plugin is now the default authentication plugin in MySQL 8.0.4 and above, based on the value of the <a undefined>default_authentication_plugin</a> system variable.

## Support in MariaDB Server

MariaDB Server does not currently support either the <a undefined>sha256_password</a> or the <a undefined>caching_sha2_password</a> authentication plugins. See [MDEV-9804](https://jira.mariadb.org/browse/MDEV-9804) for more information.

MariaDB Server does not support either of these authentication plugins. This is mainly because:

- To use the protocol, one has to distribute the server's public key to all MariaDB users, which can be cumbersome and impractical.
- The server gets the password in clear text which can cause problems if the user is convinced to connect to a malicious server.

## Client Authentication Plugins

For clients that use the [MariaDB Connector/C](/kb/en/mariadb-connector-c/) library, MariaDB provides two client authentication plugins that are compatible with MySQL's SHA-256 authentication plugins:

- `sha256_password`
- `caching_sha256_password`

When connecting with a [client or utility](/clients-utilities) to a server as a user account that authenticates with the `sha256_password` or `caching_sha256_password` authentication plugin, you may need to tell the client where to find the relevant client authentication plugin by specifying the `--plugin-dir` option. For example:

```sql
mysql --plugin-dir=/usr/local/mysql/lib64/mysql/plugin --user=alice
```

For clients that use MariaDB's `libmysqlclient` library instead of [MariaDB Connector/C](/kb/en/mariadb-connector-c/), these client authentication plugins are not supported.

### `sha256_password`

The `sha256_password` client authentication plugin is compatible with MySQL's <a undefined>sha256_password</a> authentication plugin, which was added in MySQL 5.6.

### `caching_sha256_password`

The `caching_sha256_password` client authentication plugin is compatible with MySQL's <a undefined>caching_sha2_password</a> authentication plugin, which was added in MySQL 8.0.

The `caching_sha2_password` plugin is now the default authentication plugin in MySQL 8.0.4 and above, based on the value of the <a undefined>default_authentication_plugin</a> system variable.

## Support in Client Libraries

### Using the Plugin with MariaDB Connector/C

[MariaDB Connector/C](/kb/en/mariadb-connector-c/) supports `sha256_password` and `caching_sha2_password` authentication using the [client authentication plugins](client-authentication-plugins) mentioned in the previous section.

It has supported the `sha256_password` client authentication plugin since MariaDB Connector/C 3.0.2. See [CONC-229](https://jira.mariadb.org/browse/CONC-229) for more information.

It has supported the `caching_sha256_password` client authentication plugin since MariaDB Connector/C 3.0.8 and MariaDB Connector/C 3.1.0. See [CONC-312](https://jira.mariadb.org/browse/CONC-312) for more information.

### Using the Plugin with MariaDB Connector/ODBC

[MariaDB Connector/ODBC](/kb/en/about-mariadb-connector-odbc/) does not support these authentication plugins. See [ODBC-241](https://jira.mariadb.org/browse/ODBC-241) for more information.

### Using the Plugin with MariaDB Connector/J

[MariaDB Connector/J](/kb/en/about-mariadb-connector-j/) supports `sha256_password` and `caching_sha2_password` authentication since MariaDB Connector/J 2.5.0. See [CONJ-327](https://jira.mariadb.org/browse/CONJ-327) and [CONJ-663](https://jira.mariadb.org/browse/CONJ-663) for more information.

### Using the Plugin with MariaDB Connector/Node.js

[MariaDB Connector/Node.js](/kb/en/nodejs-connector/) does not yet support these authentication plugins. See [CONJS-76](https://jira.mariadb.org/browse/CONJS-76) and [CONJS-77](https://jira.mariadb.org/browse/CONJS-77) for more information.

## See Also

- [MDEV-9804](https://jira.mariadb.org/browse/MDEV-9804) contains the plans to use if we ever decide to support these protocols.
- [History of MySQL and MariaDB authentication protocols](https://mariadb.org/history-of-mysql-mariadb-authentication-protocols)