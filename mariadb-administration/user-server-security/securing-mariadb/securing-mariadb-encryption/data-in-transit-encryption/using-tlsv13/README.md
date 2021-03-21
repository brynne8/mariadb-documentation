# Using TLSv1.3

OpenSSL 1.1.1 introduced support for TLSv1.3. TLSv1.3 is a major rewrite of the TLS protocol. Some even argued it should've been called TLSv2.0. One of the changes is that it introduces a new set of cipher suites that only work with TLSv1.3. Additionally, TLSv1.3 does not support cipher suites from previous TLS protocol versions.

This incompatible change had a non-obvious consequence. If a user had been explicitly specifying cipher suites to disable old and obsolete TLS protocol version, then that user may have also inadvertently prevented TLSv1.3 from working, unless the user remembered to add the TLSv1.3 cipher suites to their cipher list. After upgrading to OpenSSL 1.1.1, this user might believe they are using TLSv1.3, when their existing cipher suite configuration might be preventing it.

To avoid this problem, OpenSSL developers decided that TLSv1.3 cipher suites should not be affected by the normal cipher-selecting API. This means that <a undefined>ssl_cipher</a> system variable has no effect on the TLSv1.3 cipher suites.

See this [OpenSSL blog post](https://www.openssl.org/blog/blog/2018/02/08/tlsv1.3/) and [GitHub issue](https://github.com/openssl/openssl/issues/5359) for more information.

### See Also

- [Secure Connections Overview](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/secure-connections-overview)