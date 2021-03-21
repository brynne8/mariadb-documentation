# AES_ENCRYPT

## Syntax

```sql
AES_ENCRYPT(str,key_str)
```

## Description

`AES_ENCRYPT()` and [AES_DECRYPT()](/built-in-functions/secondary-functions/encryption-hashing-and-compression-functions/aes_decrypt) allow encryption and decryption of
data using the official AES (Advanced Encryption Standard) algorithm,
previously known as "Rijndael." Encoding with a 128-bit key length is
used, but you can extend it up to 256 bits by modifying the source. We
chose 128 bits because it is much faster and it is secure enough for
most purposes.

`AES_ENCRYPT()` encrypts a string <em>`str`</em> using the key <em>`key_str`</em>, and returns a binary string.

`AES_DECRYPT()` decrypts the encrypted string and returns the original
string.

The input arguments may be any length. If either argument is NULL, the result of this function is also `NULL`.

Because AES is a block-level algorithm, padding is used to encode
uneven length strings and so the result string length may be
calculated using this formula:

```sql
16 x (trunc(string_length / 16) + 1)
```

If `AES_DECRYPT()` detects invalid data or incorrect padding, it returns
`NULL`. However, it is possible for `AES_DECRYPT()` to return a non-`NULL`
value (possibly garbage) if the input data or the key is invalid.

## Examples

```sql
INSERT INTO t VALUES (AES_ENCRYPT('text',SHA2('password',512)));
```