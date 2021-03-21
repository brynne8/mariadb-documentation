# Unicode

Unicode is a standard for encoding text across multiple writing systems. [MariaDB 5.5](/kb/en/what-is-mariadb-55/) supports a number of [character sets](/kb/en/data-types-character-sets-and-collations/) for storing Unicode data:

<table><tbody><tr><th>Character Set</th><th>Description</th><th>Added</th></tr>
<tr><td>ucs2</td><td>UCS-2, each character is represented by a 2-byte code with the most significant byte first. Fixed-length 16-bit encoding.</td><td></td></tr>
<tr><td>utf8</td><td>UTF-8 encoding using one to three bytes per character. Basic Latin letters, numbers and punctuation use one byte. European and Middle East letters mostly fit into 2 bytes. Korean, Chinese, and Japanese ideographs use 3-bytes. No supplementary characters are stored.</td><td></td></tr>
<tr><td>utf8mb3</td><td>Currently an alias for utf8.</td><td><a href="/kb/en/what-is-mariadb-55/">MariaDB 5.5</a></td></tr>
<tr><td>utf8mb4</td><td>Same as utf8, but stores supplementary characters in four bytes.</td><td><a href="/kb/en/what-is-mariadb-55/">MariaDB 5.5</a></td></tr>
<tr><td>utf16</td><td>UTF-16, same as ucs2, but stores supplementary characters in 32 bits. 16 or 32-bits.</td><td><a href="/kb/en/what-is-mariadb-55/">MariaDB 5.5</a></td></tr>
<tr><td>utf32</td><td>UTF-32, fixed-length 32-bit encoding.</td><td><a href="/kb/en/what-is-mariadb-55/">MariaDB 5.5</a></td></tr>
</tbody></table>