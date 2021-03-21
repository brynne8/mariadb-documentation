# HandlerSocket Configuration Options

The [HandlerSocket](/sql-statements-structure/nosql/handlersocket) plugin has the following options.

See also the [Full list of MariaDB options, system and status variables](/mariadb-administration/variables-and-modes/full-list-of-mariadb-options-system-and-status-variables).

Add the options to the [mysqld] section of your my.cnf file.

---

### `handlersocket_accept_balance`

- <strong>Description:</strong> When set to a value other than zero ('`0`'), handlersocket will try to balance accepted connections among threads. Default is `0` but if you use persistent connections (for example if you use client-side connection pooling) then a non-zero value is recommended.
- <strong>Commandline:</strong> `--handlersocket-accept-balance="value"`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Type:</strong> number
- <strong>Range:</strong> `0` to `10000`
- <strong>Default Value:</strong> `0`

---

### `handlersocket_address`

- <strong>Description:</strong> Specify the IP address to bind to.
- <strong>Commandline:</strong> `--handlersocket-address="value"`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Type:</strong> IP Address
- <strong>Default Value:</strong> Empty, previously `0.0.0.0`

---

### `handlersocket_backlog`

- <strong>Description:</strong> Specify the listen backlog length.
- <strong>Commandline:</strong> `--handlersocket-backlog="value"`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Type:</strong> number
- <strong>Range:</strong> `5` to `1000000`
- <strong>Default Value:</strong> `32768`

---

### `handlersocket_epoll`

- <strong>Description:</strong> Specify whether to use epoll for I/O multiplexing.
- <strong>Commandline:</strong> `--handlersocket-epoll="value"`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Type:</strong> number
- <strong>Valid values:</strong>
<ul start="1"><li><strong>Min:</strong> `0`
</li><li><strong>Max:</strong> `1`
</li></ul>
- <strong>Default Value:</strong> `1`

---

### `handlersocket_plain_secret`

- <strong>Description:</strong> When set, enables plain-text authentication for the listener for read requests, with the value of the option specifying the secret authentication key.
- <strong>Commandline:</strong> `--handlersocket-plain-secret="value"`
- <strong>Dynamic:</strong> No
- <strong>Type:</strong> string
- <strong>Default Value:</strong> Empty

---

### `handlersocket_plain_secret_wr`

- <strong>Description:</strong> When set, enables plain-text authentication for the listener for write requests, with the value of the option specifying the secret authentication key.
- <strong>Commandline:</strong> `--handlersocket-plain-secret-wr="value"`
- <strong>Dynamic:</strong> No
- <strong>Type:</strong> string
- <strong>Default Value:</strong> Empty

---

### `handlersocket_port`

- <strong>Description:</strong> Specify the port to bind to for reads. An empty value disables the listener.
- <strong>Commandline:</strong> `--handlersocket-port="value"`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Type:</strong> number
- <strong>Default Value:</strong> Empty, previously `9998`

---

### `handlersocket_port_wr`

- <strong>Description:</strong> Specify the port to bind to for writes. An empty value disables the listener.
- <strong>Commandline:</strong> `--handlersocket-port-wr="value"`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Type:</strong> number
- <strong>Default Value:</strong> Empty, previously `9999`

---

### `handlersocket_rcvbuf`

- <strong>Description:</strong> Specify the maximum socket receive buffer (in bytes). If '0' then the system default is used.
- <strong>Commandline:</strong> `--handlersocket-rcvbuf="value"`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Type:</strong> number
- <strong>Range:</strong> `0` to `1677216`
- <strong>Default Value:</strong> `0`

---

### `handlersocket_readsize`

- <strong>Description:</strong> Specify the minimum length of the request buffer. Larger values consume available memory but can make handlersocket faster for large requests.
- <strong>Commandline:</strong> `--handlersocket-readsize="value"`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Type:</strong> number
- <strong>Range:</strong> `0` to `1677216`
- <strong>Default Value:</strong> `0` (possibly `4096`)

---

### `handlersocket_sndbuf`

- <strong>Description:</strong> Specify the maximum socket send buffer (in bytes). If '0' then the system default is used.
- <strong>Commandline:</strong> `--handlersocket-sndbuf="value"`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Type:</strong> number
- <strong>Range:</strong> `0` to `1677216`
- <strong>Default Value:</strong> `0`

---

### `handlersocket_threads`

- <strong>Description:</strong> Specify the number of worker threads for reads. Recommended value = ((# CPU cores) * 2).
- <strong>Commandline:</strong> `--handlersocket-threads="value"`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Type:</strong> number
- <strong>Range:</strong> `1` to `3000`
- <strong>Default Value:</strong> `16`

---

### `handlersocket_threads_wr`

- <strong>Description:</strong> Specify the number of worker threads for writes. Recommended value = 1.
- <strong>Commandline:</strong> `--handlersocket-threads-wr="value"`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Type:</strong> number
- <strong>Range:</strong> `1` to `3000`
- <strong>Default Value:</strong> `1`

---

### `handlersocket_timeout`

- <strong>Description:</strong> Specify the socket timeout in seconds.
- <strong>Commandline:</strong> `--handlersocket-timeout="value"`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Type:</strong> number
- <strong>Range:</strong> `30` to `3600`
- <strong>Default Value:</strong> `300`

---

### `handlersocket_verbose`

- <strong>Description:</strong> Specify the logging verbosity.
- <strong>Commandline:</strong> `--handlersocket-verbose="value"`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Type:</strong> number
- <strong>Valid values:</strong>
<ul start="1"><li><strong>Min:</strong> `0`
</li><li><strong>Max:</strong> `10000`
</li></ul>
- <strong>Default Value:</strong> `10`

---

### `handlersocket_wrlock_timeout`

- <strong>Description:</strong> The write lock timeout in seconds. When acting on write requests, handlersocket locks an advisory lock named 'handlersocket_wr' and this option sets the timeout for it.
- <strong>Commandline:</strong> `--handlersocket-wrlock-timeout="value"`
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Type:</strong> number
- <strong>Range:</strong> `0` to `3600`<code>
</code>

---