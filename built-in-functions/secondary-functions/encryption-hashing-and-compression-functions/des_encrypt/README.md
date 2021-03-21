# DES_ENCRYPT

## Syntax

```sql
DES_ENCRYPT(str[,{key_num|key_str}])
```

## Description

Encrypts the string with the given key using the Triple-DES algorithm.

This function works only if MariaDB has been configured with [TLS support](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/secure-connections-overview).

The encryption key to use is chosen based on the second argument to
`DES_ENCRYPT()`, if one was given. With no argument, the first key from
the DES key file is used. With a <em>`key_num`</em> argument, the given key 
number (0-9) from the DES key file is used. With a <em>`key_str`</em> argument,
the given key string is used to encrypt <em>`str`</em>.

The key file can be specified with the `--des-key-file` server option.

The return string is a binary string where the first character is 
`CHAR(128 | key_num)`. If an error occurs, `DES_ENCRYPT()` returns `NULL`.

The 128 is added to make it easier to recognize an encrypted key. If
you use a string key, <em>`key_num`</em> is 127.

The string length for the result is given by this formula:

```sql
new_len = orig_len + (8 - (orig_len % 8)) + 1
```

Each line in the DES key file has the following format:

```sql
key_num des_key_str
```

Each <em>`key_num`</em> value must be a number in the range from 0 to 9. Lines in
the file may be in any order. <em>`des_key_str`</em> is the string that is used
to encrypt the message. There should be at least one space between the
number and the key. The first key is the default key that is used if
you do not specify any key argument to `DES_ENCRYPT()`.

You can tell MariaDB to read new key values from the key file with the
FLUSH DES_KEY_FILE statement. This requires the RELOAD privilege.

One benefit of having a set of default keys is that it gives
applications a way to check for the existence of encrypted column
values, without giving the end user the right to decrypt those values.

## Examples

```sql
SELECT customer_address FROM customer_table 
   WHERE crypted_credit_card = DES_ENCRYPT('credit_card_number');
```

## See Also

- [DES_DECRYPT()](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/des_decrypt)