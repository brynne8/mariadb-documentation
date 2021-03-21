# Securing MariaDB

This section is about securing your MariaDB installation. If you are looking
for the list of security vulnerabilities fixed in MariaDB, see
[Security Vulnerabilities Fixed in MariaDB](/kb/en/cve/).

There are a number of issues to consider when looking at improving the security
of your MariaDB installation. These include:

- [Encryption](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/) — MariaDB supports encryption for data while at rest and while in transit.
- [Running mysqld as root](/mariadb-administration/user-server-security/securing-mariadb/running-mysqld-as-root/) — MariaDB should never normally be run as root
- [mysql_secure_installation](/clients-utilities/mysql_secure_installation/) — Improve the security of a MariaDB installation.
- [SecuRich](/mariadb-administration/user-server-security/user-account-management/roles/securich/) — Library of security-related stored procedures.
- [SELinux](/mariadb-administration/user-server-security/securing-mariadb/selinux/) — Security-Enhanced Linux (SELinux) is a Linux kernel module that provides a ...