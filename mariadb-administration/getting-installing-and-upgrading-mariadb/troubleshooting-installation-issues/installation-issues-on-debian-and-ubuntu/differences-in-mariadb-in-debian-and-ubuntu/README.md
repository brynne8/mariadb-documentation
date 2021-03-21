# Differences in MariaDB in Debian (and Ubuntu)

The [.deb](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-deb-files) packages provided by MariaDB Foundation's and MariaDB Corporation's repositories are not identical to the official [.deb](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-deb-files) packages provided by Debian's and Ubuntu's default repositories.

The packages provided by MariaDB Foundation's and MariaDB Corporation's repositories are generated using the Debian packaging in MariaDB's official [source code](/kb/en/getting-the-mariadb-source-code/). The Debian packaging scripts are specifically in the `debian/` directory.

The packages provided by Debian's and Ubuntu's default repositories are generated using the Debian packaging in Debian's mirror of MariaDB's source code, which contains some custom changes. The source tree can be found here:

- [https://anonscm.debian.org/cgit/pkg-mysql/mariadb-10.1.git/tree/debian](https://anonscm.debian.org/cgit/pkg-mysql/mariadb-10.1.git/tree/debian)

As a consequence, MariaDB behaves a bit differently if it is installed from Debian's and Ubuntu's default repositories.

## Option File Locations

- The [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files) located at `/etc/mysql/my.cnf` is handled by the <a undefined>update-alternatives</a> mechanism when the `mysql-common` package is installed. It is a symbolic link that references either `mysql.cnf` or `mariadb.cnf` depending on whether MySQL or MariaDB is installed. Most of the MariaDB [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files) are therefore actually located in `/etc/mysql/mariadb.d/`.

## System Variables

<table><tbody><tr><th>Variable</th><th>MariaDB in Debian</th><th>Standard MariaDB</th><th>Notes</th></tr>
<tr><td><a href="/kb/en/server-system-variables/#character_set_server">character_set_server</a></td><td>utf8mb4</td><td>latin1</td><td>Debian sets a default character set that can support emojis etc.</td></tr>
<tr><td><a href="/kb/en/server-system-variables/#collation_server">collation_server</a></td><td>utf8mb4_general_ci</td><td>latin1_swedish_ci</td><td></td></tr>
</tbody></table>

## Options

<table><tbody><tr><th>Option</th><th>MariaDB in Debian</th><th>Standard MariaDB</th><th>Notes</th></tr>
<tr><td><code><a href="/kb/en/mysqld-options/#-plugin-load-add">plugin-load-add</a></code></td><td><code>auth_socket.so</code></td><td>-</td><td>Before <a href="/kb/en/mariadb-1043-release-notes/">MariaDB 10.4.3</a>, MariaDB did not enable the <code><a href="/kb/en/authentication-plugin-unix-socket/">unix_socket</a></code> authentication plugin by default.This is default in Debian, allowing passwordless login.</td></tr>
</tbody></table>

## TLS

- MariaDB binaries from [.deb](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-deb-files) packages provided by Debian's and Ubuntu's default repositories are linked with a different TLS library than MariaDB binaries from [.deb](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-deb-files) packages provided by MariaDB Foundation's and MariaDB Corporation's repositories.

- MariaDB Server binaries:
<ul start="1"><li>In [MariaDB 10.4.6](/kb/en/mariadb-1046-release-notes/) and later, MariaDB Server is statically linked with the bundled [wolfSSL](https://www.wolfssl.com/products/wolfssl/) libraries in [.deb](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-deb-files) packages provided by Debian's and Ubuntu's default repositories.
</li><li>In [MariaDB 10.4.5](/kb/en/mariadb-1045-release-notes/) and before, MariaDB Server is statically linked with the bundled [yaSSL](https://www.wolfssl.com/products/yassl/) libraries in [.deb](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-deb-files) packages provided by Debian's and Ubuntu's default repositories.
</li><li>In contrast, MariaDB Server is dynamically linked with the system's [OpenSSL](https://www.openssl.org/) libraries in [.deb](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-deb-files) packages provided by MariaDB Foundation and MariaDB Corporation.
</li></ul>

- MariaDB [client and utility](/clients-utilities) binaries:
<ul start="1"><li>In [MariaDB 10.4.6](/kb/en/mariadb-1046-release-notes/) and later, MariaDB's [clients and utilities](/clients-utilities) and [MariaDB Connector/C](/kb/en/mariadb-connector-c/) are dynamically linked with the system's [GnuTLS](https://www.gnutls.org/) libraries in [.deb](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-deb-files) packages provided by Debian's and Ubuntu's default repositories. [libmysqlclient](https://dev.mysql.com/doc/refman/5.5/en/c-api.html) is still statically linked with the bundled [wolfSSL](https://www.wolfssl.com/products/wolfssl/) libraries.
</li><li>In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, MariaDB's [clients and utilities](/clients-utilities) and [MariaDB Connector/C](/kb/en/mariadb-connector-c/) are dynamically linked with the system's [GnuTLS](https://www.gnutls.org/) libraries in [.deb](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-deb-files) packages provided by Debian's and Ubuntu's default repositories. [libmysqlclient](https://dev.mysql.com/doc/refman/5.5/en/c-api.html) is still statically linked with the bundled [yaSSL](https://www.wolfssl.com/products/yassl/) libraries.
</li><li>In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and earlier, MariaDB's [clients and utilities](/clients-utilities) and [libmysqlclient](https://dev.mysql.com/doc/refman/5.5/en/c-api.html) are statically linked with the bundled [yaSSL](https://www.wolfssl.com/products/yassl/) libraries in [.deb](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-deb-files) packages provided by Debian's and Ubuntu's default repositories. 
</li><li>In contrast, MariaDB's [clients and utilities](/clients-utilities), [libmysqlclient](https://dev.mysql.com/doc/refman/5.5/en/c-api.html), and [MariaDB Connector/C](/kb/en/mariadb-connector-c/) are dynamically linked with the system's [OpenSSL](https://www.openssl.org/) libraries in [.deb](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-deb-files) packages provided by MariaDB Foundation's and MariaDB Corporation's repositories.
</li></ul>

- See [TLS and Cryptography Libraries Used by MariaDB](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/tls-and-cryptography-libraries-used-by-mariadb) for more information about which libraries are used on which platforms.

## Authentication

- The [unix_socket](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-unix-socket) authentication plugin is installed by default in <strong>new installations</strong> that use the [.deb](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-deb-files) packages provided by Debian's default repositories in Debian 9 and later and Ubuntu's default repositories in Ubuntu 15.10 and later.

- The `root@localhost` created by [mysql_install_db](/clients-utilities/mysql_install_db) will also be created to authenticate via the [unix_socket](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-unix-socket) authentication plugin in these builds.

## See Also

- [Moving from MySQL to MariaDB in Debian 9](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-mariadb-upgrading-from-mysql-to-mariadb/moving-from-mysql-to-mariadb-in-debian-9)

## More Information

For details, check out the Debian and Ubuntu official repositories:

- [https://packages.debian.org/search?keywords=mariadb-server&amp;searchon=names&amp;suite=all&amp;section=all](https://packages.debian.org/search?keywords=mariadb-server&amp;searchon=names&amp;suite=all&amp;section=all)
- [http://packages.ubuntu.com/search?keywords=mariadb-server&amp;searchon=names&amp;suite=all&amp;section=all](http://packages.ubuntu.com/search?keywords=mariadb-server&amp;searchon=names&amp;suite=all&amp;section=all)