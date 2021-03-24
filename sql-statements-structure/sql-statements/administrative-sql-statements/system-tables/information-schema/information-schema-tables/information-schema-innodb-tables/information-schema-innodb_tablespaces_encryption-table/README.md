# Information Schema INNODB_TABLESPACES_ENCRYPTION Table

##### MariaDB starting with [10.1.3](/kb/en/mariadb-1013-release-notes/)

Encryption of tables and tablespaces was added in [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/).

The [Information Schema](/kb/en/information_schema/) `INNODB_TABLESPACES_ENCRYPTION` table contains metadata about [encrypted InnoDB tablespaces](/kb/en/encrypting-data-for-innodb-xtradb/). When you [enable encryption for an InnoDB tablespace](/kb/en/encrypting-data-for-innodb-xtradb/#enabling-encryption), an entry for the tablespace is added to this table.  If you later [disable encryption for the InnoDB tablespace](/kb/en/encrypting-data-for-innodb-xtradb/#disabling-encryption), then the row still remains in this table, but the `ENCRYPTION_SCHEME` and `CURRENT_KEY_VERSION` columns will be set to `0`.

Viewing this table requires the [PROCESS](/kb/en/grant/#global-privileges) privilege, although a bug in versions before [MariaDB 10.1.46](/kb/en/mariadb-10146-release-notes/), [10.2.33](/kb/en/mariadb-10233-release-notes/), [10.3.24](/kb/en/mariadb-10324-release-notes/), [10.4.14](/kb/en/mariadb-10414-release-notes/) and [10.5.5](/kb/en/mariadb-1055-release-notes/) mean the [SUPER](/kb/en/grant/#global-privileges) privilege was required ([MDEV-23003](https://jira.mariadb.org/browse/MDEV-23003)).

It has the following columns:

<table><tbody><tr><th>Column</th><th>Description</th><th>Added</th></tr>
<tr><td><code>SPACE</code></td><td>InnoDB tablespace ID.</td><td></td></tr>
<tr><td><code>NAME</code></td><td>Path to the InnoDB tablespace file, without the extension.</td><td></td></tr>
<tr><td><code>ENCRYPTION_SCHEME</code></td><td>Key derivation algorithm. Only <code>1</code> is currently used to represent an algorithm. If this value is <code>0</code>, then the tablespace is unencrypted.</td><td></td></tr>
<tr><td><code>KEYSERVER_REQUESTS</code></td><td>Number of times InnoDB has had to request a key from the <a href="/kb/en/encryption-key-management/">encryption key management plugin</a>. The three most recent keys are cached internally.</td><td></td></tr>
<tr><td><code>MIN_KEY_VERSION</code></td><td>Minimum key version used to encrypt a page in the tablespace. Different pages may be encrypted with different key versions.</td><td></td></tr>
<tr><td><code>CURRENT_KEY_VERSION</code></td><td>Key version that will be used to encrypt pages. If this value is <code>0</code>, then the tablespace is unencrypted.</td><td></td></tr>
<tr><td><code>KEY_ROTATION_PAGE_NUMBER</code></td><td>Page that a <a href="/kb/en/encrypting-data-for-innodb-xtradb/#background-encryption-threads">background encryption thread</a> is currently rotating. If key rotation is not enabled, then the value will be <code>NULL</code>.</td><td></td></tr>
<tr><td><code>KEY_ROTATION_MAX_PAGE_NUMBER</code></td><td>When a <a href="/kb/en/encrypting-data-for-innodb-xtradb/#background-encryption-threads">background encryption thread</a> starts rotating a tablespace, the field contains its current size. If key rotation is not enabled, then the value will be <code>NULL</code>.</td><td></td></tr>
<tr><td><code>CURRENT_KEY_ID</code></td><td>Key ID for the encryption key currently in use.</td><td><a href="/kb/en/mariadb-10113-release-notes/">MariaDB 10.1.13</a></td></tr>
<tr><td><code>ROTATING_OR_FLUSHING</code></td><td>Current key rotation status. If this value is <code>1</code>, then the <a href="/kb/en/encrypting-data-for-innodb-xtradb/#background-encryption-threads">background encryption threads</a> are working on the tablespace. See <a href="https://jira.mariadb.org/browse/MDEV-11738">MDEV-11738</a>.</td><td><a href="/kb/en/mariadb-1025-release-notes/">MariaDB 10.2.5</a>, <a href="/kb/en/mariadb-10123-release-notes/">MariaDB 10.1.23</a></td></tr>
</tbody></table>

When the [InnoDB system tablespace](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-system-tablespaces/) is encrypted, it is represented in this table with the special name: `innodb_system`.

## Example

```sql
SELECT * FROM information_schema.INNODB_TABLESPACES_ENCRYPTION 
WHERE NAME LIKE 'db_encrypt%';
+-------+----------------------------------------------+-------------------+--------------------+-----------------+---------------------+--------------------------+------------------------------+
| SPACE | NAME                                         | ENCRYPTION_SCHEME | KEYSERVER_REQUESTS | MIN_KEY_VERSION | CURRENT_KEY_VERSION | KEY_ROTATION_PAGE_NUMBER | KEY_ROTATION_MAX_PAGE_NUMBER |
+-------+----------------------------------------------+-------------------+--------------------+-----------------+---------------------+--------------------------+------------------------------+
|    18 | db_encrypt/t_encrypted_existing_key          |                 1 |                  1 |               1 |                   1 |                     NULL |                         NULL |
|    19 | db_encrypt/t_not_encrypted_existing_key      |                 1 |                  0 |               1 |                   1 |                     NULL |                         NULL |
|    20 | db_encrypt/t_not_encrypted_non_existing_key  |                 1 |                  0 |      4294967295 |          4294967295 |                     NULL |                         NULL |
|    21 | db_encrypt/t_default_encryption_existing_key |                 1 |                  1 |               1 |                   1 |                     NULL |                         NULL |
|    22 | db_encrypt/t_encrypted_default_key           |                 1 |                  1 |               1 |                   1 |                     NULL |                         NULL |
|    23 | db_encrypt/t_not_encrypted_default_key       |                 1 |                  0 |               1 |                   1 |                     NULL |                         NULL |
|    24 | db_encrypt/t_defaults                        |                 1 |                  1 |               1 |                   1 |                     NULL |                         NULL |
+-------+----------------------------------------------+-------------------+--------------------+-----------------+---------------------+--------------------------+------------------------------+
7 rows in set (0.00 sec)
```

## See Also

- [Encrypting Data for InnoDB / XtraDB](/kb/en/encrypting-data-for-innodb-xtradb/)
- [Data at Rest Encryption](/kb/en/data-at-rest-encryption/)
- [Why Encrypt MariaDB Data?](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/why-encrypt-mariadb-data/)
- [Encryption Key Management](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/key-management-and-encryption-plugins/encryption-key-management/)