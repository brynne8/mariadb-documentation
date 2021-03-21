# CHANGE MASTER TO

The terms <em>master</em> and <em>slave</em> have historically been used in replication, but the terms terms <em>primary</em> and <em>replica</em> are now preferred. The old terms are used throughout the documentation, and in MariaDB commands, although [MariaDB 10.5](/kb/en/what-is-mariadb-105/) has begun the process of renaming. The documentation will follow over time. See [MDEV-18777](https://jira.mariadb.org/browse/MDEV-18777) to follow progress on this effort.

## Syntax

```sql
CHANGE MASTER ['connection_name'] TO master_def [, master_def] ...

master_def:
    MASTER_BIND = 'interface_name'
  | MASTER_HOST = 'host_name'
  | MASTER_USER = 'user_name'
  | MASTER_PASSWORD = 'password'
  | MASTER_PORT = port_num
  | MASTER_CONNECT_RETRY = interval
  | MASTER_HEARTBEAT_PERIOD = interval
  | MASTER_LOG_FILE = 'master_log_name'
  | MASTER_LOG_POS = master_log_pos
  | RELAY_LOG_FILE = 'relay_log_name'
  | RELAY_LOG_POS = relay_log_pos
  | MASTER_DELAY = interval
  | MASTER_SSL = {0|1}
  | MASTER_SSL_CA = 'ca_file_name'
  | MASTER_SSL_CAPATH = 'ca_directory_name'
  | MASTER_SSL_CERT = 'cert_file_name'
  | MASTER_SSL_CRL = 'crl_file_name'
  | MASTER_SSL_CRLPATH = 'crl_directory_name'
  | MASTER_SSL_KEY = 'key_file_name'
  | MASTER_SSL_CIPHER = 'cipher_list'
  | MASTER_SSL_VERIFY_SERVER_CERT = {0|1}
  | MASTER_USE_GTID = {current_pos|slave_pos|no}
  | IGNORE_SERVER_IDS = (server_id_list)
  | DO_DOMAIN_IDS = ([N,..])
  | IGNORE_DOMAIN_IDS = ([N,..])
```

## Description

The `CHANGE MASTER` statement sets the options that a [replica](/replication/) uses to connect to and replicate from a [primary](/replication/).

##### MariaDB until [10.0.7](/kb/en/mariadb-1007-release-notes/)

In [MariaDB 10.0.7](/kb/en/mariadb-1007-release-notes/) and before, the [relay_log_purge](/kb/en/replication-and-binary-log-server-system-variables/#relay_log_purge) system variable was silently set to `0` when `CHANGE MASTER` was executed.

## Multi-Source Replication

If you are using [multi-source replication](/replication/standard-replication/multi-source-replication/), then you need to specify a connection name when you execute `CHANGE MASTER`. There are two ways to do this:

- Setting the [default_master_connection](/kb/en/replication-and-binary-log-server-system-variables/#default_master_connection) system variable prior to executing `CHANGE MASTER`.
- Setting the `connection_name` parameter when executing `CHANGE MASTER`.

### `default_master_connection`

```sql
SET default_master_connection = 'gandalf';
STOP SLAVE;
CHANGE MASTER TO 
   MASTER_PASSWORD='new3cret';
START SLAVE;
```

### `connection_name`

```sql
STOP SLAVE 'gandalf';
CHANGE MASTER 'gandalf' TO 
   MASTER_PASSWORD='new3cret';
START SLAVE 'gandalf';
```

## Options

### Connection Options

#### `MASTER_USER`

The `MASTER_USER` option for `CHANGE MASTER` defines the user account that the [replica](/replication/) will use to connect to the [primary](/replication/).

This user account will need the [REPLICATION SLAVE](/kb/en/grant/#replication-slave) privilege (or, from [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/), the [REPLICATION REPLICA](/kb/en/grant/#replication-replica) on the primary.

For example:

```sql
STOP SLAVE;
CHANGE MASTER TO
   MASTER_USER='repl',
   MASTER_PASSWORD='new3cret';
START SLAVE;
```

#### `MASTER_PASSWORD`

The `MASTER_USER` option for `CHANGE MASTER` defines the password that the [replica](/replication/) will use to connect to the [primary](/replication/) as the user account defined by the [MASTER_USER](#master_user) option.

For example:

```sql
STOP SLAVE;
CHANGE MASTER TO 
   MASTER_PASSWORD='new3cret';
START SLAVE;
```

#### `MASTER_HOST`

The `MASTER_HOST` option for `CHANGE MASTER` defines the hostname or IP address of the [primary](/replication/).

If you set the value of the `MASTER_HOST` option to the empty string, then that is not the same as not setting the option's value at all. If you set the value of the `MASTER_HOST` option to the empty string, then the `CHANGE MASTER` command will fail with an error. In [MariaDB 5.3](/kb/en/what-is-mariadb-53/) and before, if you set the value of the `MASTER_HOST` option to the empty string, then the `CHANGE MASTER` command would succeed, but the subsequent [START SLAVE](/kb/en/start-slave/) command would fail.

For example:

```sql
STOP SLAVE;
CHANGE MASTER TO
   MASTER_HOST='dbserver1.example.com',
   MASTER_USER='repl',
   MASTER_PASSWORD='new3cret',
   MASTER_USE_GTID=slave_pos;
START SLAVE;
```

If you set the value of the `MASTER_HOST` option in a `CHANGE MASTER` command, then the replica assumes that the primary is different from before, even if you set the value of this option to the same value it had previously. In this scenario, the replica will consider the old values for the primary's [binary
log](/mariadb-administration/server-monitoring-logs/binary-log/) file name and position to be invalid for the new primary. As a side effect, if you do not explicitly set the values of the [MASTER_LOG_FILE](#master_log_file) and [MASTER_LOG_POS](#master_log_pos) options in the statement, then the statement will be implicitly appended with `MASTER_LOG_FILE=''` and `MASTER_LOG_POS=4`. However, if you enable [GTID](/replication/standard-replication/gtid/) mode for replication by setting the [MASTER_USE_GTID](#master_use_gtid) option to some value other than `no` in the statement, then these values will effectively be ignored anyway.

Replicas cannot connect to primaries using Unix socket files or Windows named pipes. The replica must connect to the primary using TCP/IP.

#### `MASTER_PORT`

The `MASTER_PORT` option for `CHANGE MASTER` defines the TCP/IP port of the [primary](/replication/).

For example:

```sql
STOP SLAVE;
CHANGE MASTER TO
   MASTER_HOST='dbserver1.example.com',
   MASTER_PORT=3307,
   MASTER_USER='repl',
   MASTER_PASSWORD='new3cret',
   MASTER_USE_GTID=slave_pos;
START SLAVE;
```

If you set the value of the `MASTER_PORT` option in a `CHANGE MASTER` command, then the replica assumes that the primary is different from before, even if you set the value of this option to the same value it had previously. In this scenario, the replica will consider the old values for the primary's [binary
log](/mariadb-administration/server-monitoring-logs/binary-log/) file name and position to be invalid for the new primary. As a side effect, if you do not explicitly set the values of the [MASTER_LOG_FILE](#master_log_file) and [MASTER_LOG_POS](#master_log_pos) options in the statement, then the statement will be implicitly appended with `MASTER_LOG_FILE=''` and `MASTER_LOG_POS=4`. However, if you enable [GTID](/replication/standard-replication/gtid/) mode for replication by setting the [MASTER_USE_GTID](#master_use_gtid) option to some value other than `no` in the statement, then these values will effectively be ignored anyway.

Replicas cannot connect to primaries using Unix socket files or Windows named pipes. The replica must connect to the primary using TCP/IP.

#### `MASTER_CONNECT_RETRY`

The `MASTER_CONNECT_RETRY` option for `CHANGE MASTER` defines how many seconds that the replica will wait between connection retries. The default is `60`.

```sql
STOP SLAVE;
CHANGE MASTER TO 
   MASTER_CONNECT_RETRY=20;
START SLAVE;
```

The number of connection attempts is limited by the [master_retry_count](/kb/en/mysqld-options/#-master-retry-count) option. It can be set either on the command-line or in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up the server. For example:

```sql
[mariadb]
...
master_retry_count=4294967295
```

#### `MASTER_BIND`

The `MASTER_BIND` option for `CHANGE MASTER` is only supported by MySQL 5.6.2 and later and by MySQL NDB Cluster 7.3.1 and later. This option is not yet supported by MariaDB. See [MDEV-19248](https://jira.mariadb.org/browse/MDEV-19248) for more information.

The `MASTER_BIND` option for `CHANGE MASTER` can be used on replicas that have multiple network interfaces to choose which network interface the replica will use to connect to the primary.

#### `MASTER_HEARTBEAT_PERIOD`

The `MASTER_HEARTBEAT_PERIOD` option for `CHANGE MASTER` can be used to set the interval in seconds between replication heartbeats. Whenever the primary's [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) is updated with an event, the waiting period for the next heartbeat is reset.

This option's <em>interval</em> argument has the following characteristics:

- It is a decimal value with a range of `0` to `4294967` seconds.
- It has a resolution of hundredths of a second.
- Its smallest valid non-zero value is `0.001`.
- Its default value is the value of the [slave_net_timeout](/kb/en/replication-and-binary-log-server-system-variables/#slave_net_timeout) system variable divided by 2.
- If it's set to `0`, then heartbeats are disabled.

Heartbeats are sent by the primary only if there are no unsent events in the binary log file for a period longer than the interval.

If the [RESET SLAVE](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/reset-slave-connection_name/) statement is executed, then the heartbeat interval is reset to the default.

If the [slave_net_timeout](/kb/en/replication-and-binary-log-server-system-variables/#slave_net_timeout) system variable is set to a value that is lower than the current heartbeat interval, then a warning will be issued.

### TLS Options

The TLS options are used for providing information about [TLS](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/). The options can be set even on replicas that are compiled without TLS support. The TLS options are saved to either the default `master.info` file or the file that is configured by the [master_info_file](/kb/en/mysqld-options/#-master-info-file) option, but these TLS options are ignored unless the replica supports TLS.

See [Replication with Secure Connections](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/replication-with-secure-connections/) for more information.

#### `MASTER_SSL`

The `MASTER_SSL` option for `CHANGE MASTER` tells the replica whether to force [TLS](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/) for the connection. The valid values are `0` or `1`.

For example:

```sql
STOP SLAVE;
CHANGE MASTER TO
   MASTER_SSL=1;
START SLAVE;
```

#### `MASTER_SSL_CA`

The `MASTER_SSL_CA` option for `CHANGE MASTER` defines a path to a PEM file that should contain one or more X509 certificates for trusted Certificate Authorities (CAs) to use for [TLS](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/). This option requires that you use the absolute path, not a relative path. This option implies the [MASTER_SSL](#master_ssl) option.

For example:

```sql
STOP SLAVE;
CHANGE MASTER TO
   MASTER_SSL_CERT='/etc/my.cnf.d/certificates/server-cert.pem',
   MASTER_SSL_KEY='/etc/my.cnf.d/certificates/server-key.pem',
   MASTER_SSL_CA='/etc/my.cnf.d/certificates/ca.pem',
   MASTER_SSL_VERIFY_SERVER_CERT=1;
START SLAVE;
```

See [Secure Connections Overview: Certificate Authorities (CAs)](/kb/en/secure-connections-overview/#certificate-authorities-cas) for more information.

#### `MASTER_SSL_CAPATH`

The `MASTER_SSL_CAPATH` option for `CHANGE MASTER` defines a path to a directory that contains one or more PEM files that should each contain one X509 certificate for a trusted Certificate Authority (CA) to use for [TLS](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/). This option requires that you use the absolute path, not a relative path. The directory specified by this option needs to be run through the [openssl rehash](https://www.openssl.org/docs/man1.1.1/man1/rehash.html) command. This option implies the [MASTER_SSL](#master_ssl) option.

For example:

```sql
STOP SLAVE;
CHANGE MASTER TO
   MASTER_SSL_CERT='/etc/my.cnf.d/certificates/server-cert.pem',
   MASTER_SSL_KEY='/etc/my.cnf.d/certificates/server-key.pem',
   MASTER_SSL_CAPATH='/etc/my.cnf.d/certificates/ca/',
   MASTER_SSL_VERIFY_SERVER_CERT=1;
START SLAVE;
```

See [Secure Connections Overview: Certificate Authorities (CAs)](/kb/en/secure-connections-overview/#certificate-authorities-cas) for more information.

#### `MASTER_SSL_CERT`

The `MASTER_SSL_CERT` option for `CHANGE MASTER` defines a path to the X509 certificate file to use for [TLS](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/). This option requires that you use the absolute path, not a relative path. This option implies the [MASTER_SSL](#master_ssl) option.

For example:

```sql
STOP SLAVE;
CHANGE MASTER TO
   MASTER_SSL_CERT='/etc/my.cnf.d/certificates/server-cert.pem',
   MASTER_SSL_KEY='/etc/my.cnf.d/certificates/server-key.pem',
   MASTER_SSL_CA='/etc/my.cnf.d/certificates/ca.pem',
   MASTER_SSL_VERIFY_SERVER_CERT=1;
START SLAVE;
```

#### `MASTER_SSL_CRL`

The `MASTER_SSL_CRL` option for `CHANGE MASTER` defines a path to a PEM file that should contain one or more revoked X509 certificates to use for [TLS](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/). This option requires that you use the absolute path, not a relative path.

This option is only supported if the server was built with OpenSSL. If the server was built with yaSSL, then this option is not supported. See [TLS and Cryptography Libraries Used by MariaDB](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/tls-and-cryptography-libraries-used-by-mariadb/) for more information about which libraries are used on which platforms.

For example:

```sql
STOP SLAVE;
CHANGE MASTER TO
   MASTER_SSL_CERT='/etc/my.cnf.d/certificates/server-cert.pem',
   MASTER_SSL_KEY='/etc/my.cnf.d/certificates/server-key.pem',
   MASTER_SSL_CA='/etc/my.cnf.d/certificates/ca.pem',
   MASTER_SSL_VERIFY_SERVER_CERT=1,
   MASTER_SSL_CRL='/etc/my.cnf.d/certificates/crl.pem';
START SLAVE;
```

See [Secure Connections Overview: Certificate Revocation Lists (CRLs)](/kb/en/secure-connections-overview/#certificate-revocation-lists-crls) for more information.

#### `MASTER_SSL_CRLPATH`

The `MASTER_SSL_CRLPATH` option for `CHANGE MASTER` defines a path to a directory that contains one or more PEM files that should each contain one revoked X509 certificate to use for [TLS](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/). This option requires that you use the absolute path, not a relative path. The directory specified by this variable needs to be run through the [openssl rehash](https://www.openssl.org/docs/man1.1.1/man1/rehash.html) command.

This option is only supported if the server was built with OpenSSL. If the server was built with yaSSL, then this option is not supported. See [TLS and Cryptography Libraries Used by MariaDB](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/tls-and-cryptography-libraries-used-by-mariadb/) for more information about which libraries are used on which platforms.

For example:

```sql
STOP SLAVE;
CHANGE MASTER TO
   MASTER_SSL_CERT='/etc/my.cnf.d/certificates/server-cert.pem',
   MASTER_SSL_KEY='/etc/my.cnf.d/certificates/server-key.pem',
   MASTER_SSL_CA='/etc/my.cnf.d/certificates/ca.pem',
   MASTER_SSL_VERIFY_SERVER_CERT=1,
   MASTER_SSL_CRLPATH='/etc/my.cnf.d/certificates/crl/';
START SLAVE;
```

See [Secure Connections Overview: Certificate Revocation Lists (CRLs)](/kb/en/secure-connections-overview/#certificate-revocation-lists-crls) for more information.

#### `MASTER_SSL_KEY`

The `MASTER_SSL_KEY` option for `CHANGE MASTER` defines a path to a private key file to use for [TLS](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/). This option requires that you use the absolute path, not a relative path. This option implies the [MASTER_SSL](#master_ssl) option.

For example:

```sql
STOP SLAVE;
CHANGE MASTER TO
   MASTER_SSL_CERT='/etc/my.cnf.d/certificates/server-cert.pem',
   MASTER_SSL_KEY='/etc/my.cnf.d/certificates/server-key.pem',
   MASTER_SSL_CA='/etc/my.cnf.d/certificates/ca.pem',
   MASTER_SSL_VERIFY_SERVER_CERT=1;
START SLAVE;
```

#### `MASTER_SSL_CIPHER`

The `MASTER_SSL_CIPHER` option for `CHANGE MASTER` defines the list of permitted ciphers or cipher suites to use for [TLS](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/). Besides cipher names, if MariaDB was compiled with OpenSSL, this option could be set to "SSLv3" or "TLSv1.2" to allow all SSLv3 or all TLSv1.2 ciphers. Note that the TLSv1.3 ciphers cannot be excluded when using OpenSSL, even by using this option. See [Using TLSv1.3](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/using-tlsv13/) for details. This option implies the [MASTER_SSL](#master_ssl) option.

For example:

```sql
STOP SLAVE;
CHANGE MASTER TO
   MASTER_SSL_CERT='/etc/my.cnf.d/certificates/server-cert.pem',
   MASTER_SSL_KEY='/etc/my.cnf.d/certificates/server-key.pem',
   MASTER_SSL_CA='/etc/my.cnf.d/certificates/ca.pem',
   MASTER_SSL_VERIFY_SERVER_CERT=1,
   MASTER_SSL_CIPHER='TLSv1.2';
START SLAVE;
```

#### `MASTER_SSL_VERIFY_SERVER_CERT`

The `MASTER_SSL_VERIFY_SERVER_CERT` option for `CHANGE MASTER` enables [server certificate verification](/kb/en/secure-connections-overview/#server-certificate-verification). This option is disabled by default.

For example:

```sql
STOP SLAVE;
CHANGE MASTER TO
   MASTER_SSL_CERT='/etc/my.cnf.d/certificates/server-cert.pem',
   MASTER_SSL_KEY='/etc/my.cnf.d/certificates/server-key.pem',
   MASTER_SSL_CA='/etc/my.cnf.d/certificates/ca.pem',
   MASTER_SSL_VERIFY_SERVER_CERT=1;
START SLAVE;
```

See [Secure Connections Overview: Server Certificate Verification](/kb/en/secure-connections-overview/#server-certificate-verification) for more information.

### Binary Log Options

These options are related to the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) position on the primary.

#### `MASTER_LOG_FILE`

The `MASTER_LOG_FILE` option for `CHANGE MASTER` can be used along with `MASTER_LOG_POS` to specify the coordinates at which the [replica's I/O thread](/kb/en/replication-threads/#slave-io-thread) should begin reading from the primary's [binary logs](/mariadb-administration/server-monitoring-logs/binary-log/) the next time the thread starts.

For example:

```sql
STOP SLAVE;
CHANGE MASTER TO
   MASTER_LOG_FILE='master2-bin.001',
   MASTER_LOG_POS=4;
START SLAVE;
```

The [MASTER_LOG_FILE](#master_log_file) and [MASTER_LOG_POS](#master_log_pos) options cannot be specified if the [RELAY_LOG_FILE](#relay_log_file) and [RELAY_LOG_POS](#relay_log_pos) options were also specified.

The [MASTER_LOG_FILE](#master_log_file) and [MASTER_LOG_POS](#master_log_pos) options are effectively ignored if you enable [GTID](/replication/standard-replication/gtid/) mode for replication by setting the [MASTER_USE_GTID](#master_use_gtid) option to some value other than `no` in the statement.

#### `MASTER_LOG_POS`

The `MASTER_LOG_POS` option for `CHANGE MASTER` can be used along with `MASTER_LOG_FILE` to specify the coordinates at which the [replica's I/O thread](/kb/en/replication-threads/#slave-io-thread) should begin reading from the primary's [binary logs](/mariadb-administration/server-monitoring-logs/binary-log/) the next time the thread starts.

For example:

```sql
STOP SLAVE;
CHANGE MASTER TO
   MASTER_LOG_FILE='master2-bin.001',
   MASTER_LOG_POS=4;
START SLAVE;
```

The [MASTER_LOG_FILE](#master_log_file) and [MASTER_LOG_POS](#master_log_pos) options cannot be specified if the [RELAY_LOG_FILE](#relay_log_file) and [RELAY_LOG_POS](#relay_log_pos) options were also specified.

The [MASTER_LOG_FILE](#master_log_file) and [MASTER_LOG_POS](#master_log_pos) options are effectively ignored if you enable [GTID](/replication/standard-replication/gtid/) mode for replication by setting the [MASTER_USE_GTID](#master_use_gtid) option to some value other than `no` in the statement.

### Relay Log Options

These options are related to the [relay log](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/) position on the replica.

#### `RELAY_LOG_FILE`

The `RELAY_LOG_FILE` option for `CHANGE MASTER` can be used along with the [RELAY_LOG_POS](#relay_log_pos) option to specify the coordinates at which the [replica's SQL thread](/kb/en/replication-threads/#slave-sql-thread) should begin reading from the [relay log](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/) the next time the thread starts.

The `CHANGE MASTER` statement usually deletes all [relay log](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/) files. However, if the `RELAY_LOG_FILE` and/or `RELAY_LOG_POS` options are specified, then existing [relay log](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/) files are kept.

When you want to change the [relay log](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/) position, you only need to stop the [replica's SQL thread](/kb/en/replication-threads/#slave-sql-thread). The [replica's I/O thread](/kb/en/replication-threads/#slave-io-thread) can continue running. The [STOP SLAVE](/kb/en/stop-slave/) and [START SLAVE](/kb/en/start-slave/) statements support the `SQL_THREAD` option for this scenario. For example:

```sql
STOP SLAVE SQL_THREAD;
CHANGE MASTER TO
   RELAY_LOG_FILE='slave-relay-bin.006',
   RELAY_LOG_POS=4025;
START SLAVE SQL_THREAD;
```

When the value of this option is changed, the metadata about the [replica's SQL thread's](/kb/en/replication-threads/#slave-sql-thread) position in the [relay logs](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/) will also be changed in the `relay-log.info` file or the file that is configured by the [relay_log_info_file](/kb/en/replication-and-binary-log-system-variables/#relay_log_info_file) system variable.

The [RELAY_LOG_FILE](#relay_log_file) and [RELAY_LOG_POS](#relay_log_pos) options cannot be specified if the [MASTER_LOG_FILE](#master_log_file) and [MASTER_LOG_POS](#master_log_pos) options were also specified.

#### `RELAY_LOG_POS`

The `RELAY_LOG_POS` option for `CHANGE MASTER` can be used along with the [RELAY_LOG_FILE](#relay_log_file) option to specify the coordinates at which the [replica's SQL thread](/kb/en/replication-threads/#slave-sql-thread) should begin reading from the [relay log](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/) the next time the thread starts.

The `CHANGE MASTER` statement usually deletes all [relay log](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/) files. However, if the `RELAY_LOG_FILE` and/or `RELAY_LOG_POS` options are specified, then existing [relay log](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/) files are kept.

When you want to change the [relay log](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/) position, you only need to stop the [replica's SQL thread](/kb/en/replication-threads/#slave-sql-thread). The [replica's I/O thread](/kb/en/replication-threads/#slave-io-thread) can continue running. The [STOP SLAVE](/kb/en/stop-slave/) and [START SLAVE](/kb/en/start-slave/) statements support the `SQL_THREAD` option for this scenario. For example:

```sql
STOP SLAVE SQL_THREAD;
CHANGE MASTER TO
   RELAY_LOG_FILE='slave-relay-bin.006',
   RELAY_LOG_POS=4025;
START SLAVE SQL_THREAD;
```

When the value of this option is changed, the metadata about the [replica's SQL thread's](/kb/en/replication-threads/#slave-sql-thread) position in the [relay logs](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/) will also be changed in the `relay-log.info` file or the file that is configured by the [relay_log_info_file](/kb/en/replication-and-binary-log-system-variables/#relay_log_info_file) system variable.

The [RELAY_LOG_FILE](#relay_log_file) and [RELAY_LOG_POS](#relay_log_pos) options cannot be specified if the [MASTER_LOG_FILE](#master_log_file) and [MASTER_LOG_POS](#master_log_pos) options were also specified.

### GTID Options

#### `MASTER_USE_GTID`

The `MASTER_USE_GTID` option for `CHANGE MASTER` can be used to configure the replica to use the [global transaction ID (GTID)](/kb/en/global-transaction-id/) when connecting to a primary. The possible values are:

- `current_pos` - Replicate in [GTID](/replication/standard-replication/gtid/) mode and use [gtid_current_pos](/kb/en/gtid/#gtid_current_pos) as the position to start downloading transactions from the primary.
- `slave_pos` - Replicate in [GTID](/replication/standard-replication/gtid/) mode and use [gtid_slave_pos](/kb/en/gtid/#gtid_slave_pos) as the position to start downloading transactions from the primary. From [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/), `replica_pos` is an alias for `slave_pos`.
- `no` - Don't replicate in [GTID](/replication/standard-replication/gtid/) mode.

For example:

```sql
STOP SLAVE;
CHANGE MASTER TO
   MASTER_USE_GTID = current_pos;
START SLAVE;
```

Or:

```sql
STOP SLAVE;
SET GLOBAL gtid_slave_pos='0-1-153';
CHANGE MASTER TO
   MASTER_USE_GTID = slave_pos;
START SLAVE;
```

### Replication Filter Options

Also see [Replication filters](/replication/standard-replication/replication-filters/).

#### `IGNORE_SERVER_IDS`

The `IGNORE_SERVER_IDS` option for `CHANGE MASTER` can be used to configure a [replica](/replication/) to ignore [binary log](binary_log) events that originated from certain servers. Filtered [binary log](binary_log) events will not get logged to the replica’s [relay log](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/), and they will not be applied by the replica.

The option's value can be specified by providing a comma-separated list of [server_id](/kb/en/replication-and-binary-log-system-variables/#server_id) values. For example:

```sql
STOP SLAVE;
CHANGE MASTER TO 
   IGNORE_SERVER_IDS = (3,5);
START SLAVE;
```

If you would like to clear a previously set list, then you can set the value to an empty list. For example:

```sql
STOP SLAVE;
CHANGE MASTER TO 
   IGNORE_SERVER_IDS = ();
START SLAVE;
```

#### `DO_DOMAIN_IDS`

##### MariaDB starting with [10.1.2](/kb/en/mariadb-1012-release-notes/)

The `DO_DOMAIN_IDS` option for `CHANGE MASTER` was first added in [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/).

The `DO_DOMAIN_IDS` option for `CHANGE MASTER` can be used to configure a [replica](/replication/) to only apply [binary log](binary_log) events if the transaction's [GTID](/kb/en/global-transaction-id/) is in a specific [gtid_domain_id](/kb/en/gtid/#gtid_domain_id) value. Filtered [binary log](binary_log) events will not get logged to the replica’s [relay log](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/), and they will not be applied by the replica.

The option's value can be specified by providing a comma-separated list of [gtid_domain_id](/kb/en/gtid/#gtid_domain_id) values. Duplicate values are automatically ignored. For example:

```sql
STOP SLAVE;
CHANGE MASTER TO 
   DO_DOMAIN_IDS = (1,2);
START SLAVE;
```

If you would like to clear a previously set list, then you can set the value to an empty list. For example:

```sql
STOP SLAVE;
CHANGE MASTER TO 
   DO_DOMAIN_IDS = ();
START SLAVE;
```

The [DO_DOMAIN_IDS](#do_domain_ids) option and the [IGNORE_DOMAIN_IDS](#ignore_domain_ids) option cannot both be set to non-empty values at the same time. If you want to set the [DO_DOMAIN_IDS](#do_domain_ids) option, and the [IGNORE_DOMAIN_IDS](#ignore_domain_ids) option was previously set, then you need to clear the value of the [IGNORE_DOMAIN_IDS](#ignore_domain_ids) option. For example:

```sql
STOP SLAVE;
CHANGE MASTER TO 
   IGNORE_DOMAIN_IDS = (), 
   DO_DOMAIN_IDS = (1,2);
START SLAVE;
```

The `DO_DOMAIN_IDS` option can only be specified if the replica is replicating in [GTID](/replication/standard-replication/gtid/) mode. Therefore, the [MASTER_USE_GTID](#master_use_gtid) option must also be set to some value other than `no` in order to use this option.

#### `IGNORE_DOMAIN_IDS`

##### MariaDB starting with [10.1.2](/kb/en/mariadb-1012-release-notes/)

The `IGNORE_DOMAIN_IDS` option for `CHANGE MASTER` was first added in [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/).

The `IGNORE_DOMAIN_IDS` option for `CHANGE MASTER` can be used to configure a [replica](/replication/) to ignore [binary log](binary_log) events if the transaction's [GTID](/kb/en/global-transaction-id/) is in a specific [gtid_domain_id](/kb/en/gtid/#gtid_domain_id) value. Filtered [binary log](binary_log) events will not get logged to the replica’s [relay log](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/), and they will not be applied by the replica.

The option's value can be specified by providing a comma-separated list of [gtid_domain_id](/kb/en/gtid/#gtid_domain_id) values. Duplicate values are automatically ignored. For example:

```sql
STOP SLAVE;
CHANGE MASTER TO 
   IGNORE_DOMAIN_IDS = (1,2);
START SLAVE;
```

If you would like to clear a previously set list, then you can set the value to an empty list. For example:

```sql
STOP SLAVE;
CHANGE MASTER TO 
   IGNORE_DOMAIN_IDS = ();
START SLAVE;
```

The [DO_DOMAIN_IDS](#do_domain_ids) option and the [IGNORE_DOMAIN_IDS](#ignore_domain_ids) option cannot both be set to non-empty values at the same time. If you want to set the [IGNORE_DOMAIN_IDS](#ignore_domain_ids) option, and the [DO_DOMAIN_IDS](#do_domain_ids) option was previously set, then you need to clear the value of the [DO_DOMAIN_IDS](#do_domain_ids) option. For example:

```sql
STOP SLAVE;
CHANGE MASTER TO 
   DO_DOMAIN_IDS = (), 
   IGNORE_DOMAIN_IDS = (1,2);
START SLAVE;
```

The `IGNORE_DOMAIN_IDS` option can only be specified if the replica is replicating in [GTID](/replication/standard-replication/gtid/) mode. Therefore, the [MASTER_USE_GTID](#master_use_gtid) option must also be set to some value other than `no` in order to use this option.

### Delayed Replication Options

#### `MASTER_DELAY`

##### MariaDB starting with [10.2.3](/kb/en/mariadb-1023-release-notes/)

The `MASTER_DELAY` option for `CHANGE MASTER` was first added in [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/) to enable [delayed replication](/replication/standard-replication/delayed-replication/).

The `MASTER_DELAY` option for `CHANGE MASTER` can be used to enable [delayed replication](/replication/standard-replication/delayed-replication/). This option specifies the time in seconds (at least) that a replica should lag behind the primary up to a maximum value of 2147483647, or about 68 years. Before executing an event, the replica will first wait, if necessary, until the given time has passed since the event was created on the primary. The result is that the replica will reflect the state of the primary some time back in the past. The default is zero, no delay.

```sql
STOP SLAVE;
CHANGE MASTER TO 
   MASTER_DELAY=3600;
START SLAVE;
```

## Changing Option Values

If you don't specify a given option when executing the `CHANGE MASTER` statement, then the option keeps its old value in most cases. Most of the time, there is no need to specify the options that do not need to change. For example, if the password for the user account that the replica uses to connect to its primary has changed, but no other options need to change, then you can just change the [MASTER_PASSWORD](#master_password) option by executing the following commands:

```sql
STOP SLAVE;
CHANGE MASTER TO 
   MASTER_PASSWORD='new3cret';
START SLAVE;
```

There are some cases where options are implicitly reset, such as when the [MASTER_HOST](#master_host) and [MASTER_PORT](#master_port) options are changed.

## Option Persistence

The values of the [MASTER_LOG_FILE](#master_log_file) and [MASTER_LOG_POS](#master_log_pos) options (i.e. the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) position on the primary) and most other options are written to either the default `master.info` file or the file that is configured by the [master_info_file](/kb/en/mysqld-options/#-master-info-file) option. The [replica's I/O thread](/kb/en/replication-threads/#slave-io-thread) keeps this [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) position updated as it downloads events only when [MASTER_USE_GTID](#master_use_gtid) option
 is set to `NO`.  Otherwise the file is not updated on a per event basis.

The [master_info_file](/kb/en/mysqld-options/#-master-info-file) option can be set either on the command-line or in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up the server. For example:

```sql
[mariadb]
...
master_info_file=/mariadb/myserver1-master.info
```

The values of the [RELAY_LOG_FILE](#relay_log_file) and [RELAY_LOG_POS](#relay_log_pos) options (i.e. the [relay log](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/) position) are written to either the default `relay-log.info` file or the file that is configured by the [relay_log_info_file](/kb/en/replication-and-binary-log-system-variables/#relay_log_info_file) system variable. The [replica's SQL thread](/kb/en/replication-threads/#slave-sql-thread) keeps this [relay log](/mariadb-administration/server-monitoring-logs/binary-log/relay-log/) position updated as it applies events.

The [relay_log_info_file](/kb/en/replication-and-binary-log-system-variables/#relay_log_info_file) system variable can be set either on the command-line or in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up the server. For example:

```sql
[mariadb]
...
relay_log_info_file=/mariadb/myserver1-relay-log.info
```

## GTID Persistence

If the replica is replicating [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) events that contain [GTIDs](/replication/standard-replication/gtid/), then the [replica's SQL thread](/kb/en/replication-threads/#slave-sql-thread) will write every GTID that it applies to the [mysql.gtid_slave_pos](/kb/en/mysqlgtid_slave_pos-table/) table. This GTID can be inspected and modified through the [gtid_slave_pos](/kb/en/gtid/#gtid_slave_pos) system variable.

If the replica has the [log_slave_updates](/kb/en/replication-and-binary-log-system-variables/#log_slave_updates) system variable enabled and if the replica has the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) enabled, then every write by the [replica's SQL thread](/kb/en/replication-threads/#slave-sql-thread) will also go into the replica's [binary log](/mariadb-administration/server-monitoring-logs/binary-log/). This means that [GTIDs](/replication/standard-replication/gtid/) of replicated transactions would be reflected in the value of the [gtid_binlog_pos](/kb/en/gtid/#gtid_binlog_pos) system variable.

## Creating a Slave from a Backup

The `CHANGE MASTER` statement is useful for setting up a replica when you have a backup of the primary and you also have the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) position or [GTID](/replication/standard-replication/gtid/) position corresponding to the backup.

After restoring the backup on the replica, you could execute something like this to use the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) position:

```sql
CHANGE MASTER TO
   MASTER_LOG_FILE='master2-bin.001',
   MASTER_LOG_POS=4;
START SLAVE;
```

Or you could execute something like this to use the [GTID](/replication/standard-replication/gtid/) position:

```sql
SET GLOBAL gtid_slave_pos='0-1-153';
CHANGE MASTER TO
   MASTER_USE_GTID=slave_pos;
START SLAVE;
```

See [Setting up a Replication Slave with Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/setting-up-a-replication-slave-with-mariabackup/) for more information on how to do this with [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/).

## Example

The following example changes the primary and primary's binary log coordinates.
This is used when you want to set up the replica to replicate the primary:

```sql
CHANGE MASTER TO
   MASTER_HOST='master2.mycompany.com',
   MASTER_USER='replication',
   MASTER_PASSWORD='bigs3cret',
   MASTER_PORT=3306,
   MASTER_LOG_FILE='master2-bin.001',
   MASTER_LOG_POS=4,
   MASTER_CONNECT_RETRY=10;
START SLAVE;
```

## See Also

- [Setting up replication](/replication/standard-replication/setting-up-replication/)
- [START SLAVE](/kb/en/start-slave/)
- [Multi-source replication](/replication/standard-replication/multi-source-replication/)
- [RESET SLAVE](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/reset-slave-connection_name/). Removes a connection created  with `CHANGE MASTER TO`.
- [Global Transaction ID](/kb/en/global-transaction-id/)