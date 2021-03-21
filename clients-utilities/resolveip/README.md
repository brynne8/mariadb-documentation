# resolveip

resolveip is a utility for resolving IP addresses to host names and vice versa.

## Usage

```sql
resolveip [OPTIONS] hostname or IP-address
```

## Options

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>-?</code>, <code>--help</code></td><td>Display help and exit.</td></tr>
<tr><td><code>-I</code>, <code>--info</code></td><td>Synonym for <code>--help</code>.</td></tr>
<tr><td><code>-s</code>, <code>--silent#</code></td><td>Be more silent.</td></tr>
<tr><td><code>-V</code>, <code>--version</code></td><td>Display version information and exit.</td></tr>
</tbody></table>

## Examples

```sql
shell> resolveip mariadb.org
IP address of mariadb.org is 166.78.144.191
```

```sql
resolveip 166.78.144.191
Host name of 166.78.144.191 is mariadb.org
```