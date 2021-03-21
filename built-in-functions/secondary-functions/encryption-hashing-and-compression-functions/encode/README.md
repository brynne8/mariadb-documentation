# ENCODE

## Syntax

```sql
ENCODE(str,pass_str)
```

## Description

ENCODE is not considered cryptographically secure, and should not be used for password encryption.

Encrypt `str` using `pass_str` as the password. To decrypt the result, use
[DECODE()](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/decode).

The result is a binary string of the same length as `str`.

The strength of the encryption is based on how good the random generator is.

It is not recommended to rely on the encryption performed by the ENCODE function. Using a salt value (changed when a password is updated) will improve matters somewhat, but for storing passwords, consider a more cryptographically secure function, such as [SHA2()](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/sha2).

## Examples

```sql
ENCODE('not so secret text', CONCAT('random_salt','password'))
```