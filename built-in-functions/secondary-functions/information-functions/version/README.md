# VERSION

## Syntax

```sql
VERSION()
```

## Description

Returns a string that indicates the MariaDB server version. The string
uses the utf8 [character set](/kb/en/data-types-character-sets-and-collations/).

## Examples

```sql
SELECT VERSION();
+----------------+
| VERSION()      |
+----------------+
| 10.4.7-MariaDB |
+----------------+
```

The <code class="fixed" style="white-space:pre-wrap">VERSION()</code> string may have one or more of the following suffixes:

<table><tbody><tr><th>Suffix</th><th>Description</th></tr>
<tr><td>-embedded</td><td>The server is an embedded server (libmysqld).</td></tr>
<tr><td>-log</td><td>General logging, slow logging or binary (replication) logging is enabled.</td></tr>
<tr><td>-debug</td><td>The server is compiled for debugging.</td></tr>
<tr><td>-valgrind</td><td>&nbsp;The server is compiled to be instrumented with valgrind.</td></tr>
</tbody></table>

## Changing the Version String

Some old legacy code may break because they are parsing the
`VERSION` string and expecting a MySQL string or a simple version
string like Joomla til API17, see [MDEV-7780](https://jira.mariadb.org/browse/MDEV-7780).

From [MariaDB 10.2](/kb/en/what-is-mariadb-102/), one can fool these applications by setting the version string from the command line or the my.cnf files with [--version=...](/kb/en/server-system-variables/#version).