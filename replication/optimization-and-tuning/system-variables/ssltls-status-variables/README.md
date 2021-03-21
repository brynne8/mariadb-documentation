# SSL/TLS Status Variables

The status variables listed on this page relate to encrypting data during transfer with the Transport Layer Security (TLS) protocol.  Often, the term Secure Socket Layer (SSL) is used interchangeably with TLS, although strictly speaking, the SSL protocol is a predecessor to TLS and is no longer considered secure.

For compatibility reasons, the TLS status variables in MariaDB still use the `Ssl_` prefix, but MariaDB only supports its more secure successors. For more information on SSL/TLS in MariaDB, see [Secure Connections Overview](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/secure-connections-overview/).

## Variables

#### `Ssl_accept_renegotiates`

- <strong>Description:</strong> Number of negotiations needed to establish the TLS connection. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Ssl_accepts`

- <strong>Description:</strong> Number of accepted TLS handshakes. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Ssl_callback_cache_hits`

- <strong>Description:</strong> Number of sessions retrieved from the session cache. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Ssl_cipher`

- <strong>Description:</strong> The TLS cipher currently in use.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `string`

---

#### `Ssl_cipher_list`

- <strong>Description:</strong> List of the available TLS ciphers.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `string`

---

#### `Ssl_client_connects`

- <strong>Description:</strong> Number of TLS handshakes started in client mode. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Ssl_connect_renegotiates`

- <strong>Description:</strong> Number of negotiations needed to establish the connection to a TLS-enabled master. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Ssl_ctx_verify_depth`

- <strong>Description:</strong> Number of tested TLS certificates in the chain. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Ssl_ctx_verify_mode`

- <strong>Description:</strong> Mode used for TLS context verification.The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Ssl_default_timeout`

- <strong>Description:</strong> Default timeout for TLS, in seconds.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Ssl_finished_accepts`

- <strong>Description:</strong> Number of successful TLS sessions in server mode. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Ssl_finished_connects`

- <strong>Description:</strong> Number of successful TLS sessions in client mode. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Ssl_server_not_after`

- <strong>Description:</strong> Last valid date for the TLS certificate.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> <a undefined>MariaDB 10.0</a>

---

#### `Ssl_server_not_before`

- <strong>Description:</strong> First valid date for the TLS certificate.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`
- <strong>Introduced:</strong> <a undefined>MariaDB 10.0</a>

---

#### `Ssl_session_cache_hits`

- <strong>Description:</strong> Number of TLS sessions found in the session cache. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Ssl_session_cache_misses`

- <strong>Description:</strong> Number of TLS sessions not found in the session cache. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Ssl_session_cache_mode`

- <strong>Description:</strong> Mode used for TLS caching by the server.
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `string`

---

#### `Ssl_session_cache_overflows`

- <strong>Description:</strong> Number of sessions removed from the session cache because it was full. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Ssl_session_cache_size`

- <strong>Description:</strong> Size of the session cache. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Ssl_session_cache_timeouts`

- <strong>Description:</strong> Number of sessions which have timed out. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Ssl_sessions_reused`

- <strong>Description:</strong> Number of sessions reused. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Ssl_used_session_cache_entries`

- <strong>Description:</strong> Current number of sessions in the session cache. The global value can be flushed by [FLUSH STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
- <strong>Scope:</strong> Global
- <strong>Data Type:</strong> `numeric`

---

#### `Ssl_verify_depth`

- <strong>Description:</strong> TLS verification depth.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Ssl_verify_mode`

- <strong>Description:</strong> TLS verification mode.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `numeric`

---

#### `Ssl_version`

- <strong>Description:</strong> TLS version in use.
- <strong>Scope:</strong> Global, Session
- <strong>Data Type:</strong> `string`

---

## See Also

- [Server Status Variables](/replication/optimization-and-tuning/system-variables/server-status-variables/)  -  complete list of status variables.
- [Full list of MariaDB options, system and status variables](/mariadb-administration/variables-and-modes/full-list-of-mariadb-options-system-and-status-variables/)