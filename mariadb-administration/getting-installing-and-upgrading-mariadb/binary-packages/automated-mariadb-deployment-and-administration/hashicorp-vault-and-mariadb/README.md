# HashiCorp Vault and MariaDB

Vault is open source software for secret management provided by HashiCorp. It is designed to avoid sharing secrets of various types, like passwords and private keys. When building automation, Vault is a good solution to avoid storing secrets in plain text in a repository.

MariaDB and Vault may relate each other in several ways:

- MariaDB secrets can be stored in Vault. Typically this includes user's passwords and private keys for SSH access.
- MariaDB (and MySQL) can be used as a secret engine, a component which stores, generates, or encrypts data.
- MariaDB (and MySQL) can be used as a backend storage, providing durability for Vault data.

For information about how to install Vault, see [Install Vault](https://www.vaultproject.io/docs/install).

## Vault Features

Vault is used via an HTTP/HTTPS API.

Vault is identity-based. Users login and Vault sends them a token that is valid for a certain amount of time, or until certain conditions occur. Users with a valid token may request to obtain secrets for which they have proper permissions.

Vault encrypts the secrets it stores.

Vault can optionally audit changes to secrets and secrets requests by the users.

## Vault Architecture

Vault is a server. This allows decoupling the secrets management logic from the clients, which only need to login and keep a token until it expires.

The sever can actually be a cluster of servers, to implement high availability.

The main Vault components are:

- <strong>Storage Backed</strong>: This is where the secrets are stored. Vault only send encrypted data to the backend storage.
- <strong>HTTP API</strong>: This API is used by the clients, and provides an access to Vault server.
- <strong>Barrier</strong>: Similarly to an actual barrier, it protects all inner Vault components. The HTTP API and the storage backend are outside of the barrier and could be accessed by anyone. All communications from and to these components have to pass through the barrier. The barrier verifies data and encrypts it. The barrier can have two states: <em>sealed</em> or <em>unsealed</em>. Data can only pass through when the barrier is unsealed. All the following components are located inside the barrier.
- <strong>Auth Method</strong>: Handles login attempts from clients. When a login succeeds, the auth method returns a list of security policies to Vault core.
- <strong>Token Store</strong>: Here the tokens generated as a result of a succeeded login are stored.
- <strong>Secrets Engines</strong>: These components manage secrets. They can have different levels of complexity. Some of them simply expect to receive a key, and return the corresponding secret. Others may generate secrets, including one-time-passwords.
- <strong>Audit Devices</strong>: These components log the requests received by Vault and the responses sent back to the clients.There may be multiple devices, in which case an <strong>Audit Broker</strong> sends the request or response to the proper device.

## Dev Mode

It is possible to start Vault in dev mode:

```sql
vault server -dev
```

Dev mode is useful for learning Vault, or running experiments on some particular features. It is extremely insecure, because dev mode is equivalent to starting Vault with several insecure options. This means that Vault should never run in production in dev mode. However, this also means that all the regular Vault features are available in dev mode.

Dev mode simplifies all operations. Actually, no configuration is necessary to get Vault up and running in dev mode. It makes it possible to communicate with the Vault API from the shell without any authentication. Data is stored in memory by default. Vault is unsealed by default, and if explicitly sealed, it can be unsealed using only one key.

For more details, see ["Dev" Server Mode](https://www.vaultproject.io/docs/concepts/dev-server) in Vault documentation.

## Vault Resources and References

- [Documentation](https://www.vaultproject.io/docs).
- [MySQL/MariaDB Database Secrets Engine](https://www.vaultproject.io/docs/secrets/databases/mysql-maria).
- [MySQL Storage Backend](https://www.vaultproject.io/docs/configuration/storage/mysql).

---

Content initially contributed by [Vettabase Ltd](https://vettabase.com/).