# mysqldumpslow

##### MariaDB starting with [10.4.6](/kb/en/mariadb-1046-release-notes/)

From [MariaDB 10.4.6](/kb/en/mariadb-1046-release-notes/), `mariadb-dumpslow` is a symlink to `mysqldumpslow`.

##### MariaDB starting with [10.5.2](/kb/en/mariadb-1052-release-notes/)

From [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), `mariadb-dumpslow` is the name of the tool, with `mysqldumpslow` a symlink .

`mysqldumpslow` is a tool to examine the [slow query log](/mariadb-administration/server-monitoring-logs/slow-query-log/).

It parses the slow query log files, printing a summary result. Normally, mysqldumpslow groups queries that are similar except for the particular values of number and string data values. It “abstracts” these values to N and ´S´ when displaying summary output. The `-a` and `-n` options can be used to modify value abstracting behavior.

## Usage

```sql
mysqldumpslow [ options... ] [ logs... ]
```

## Options

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>-a</code></td><td>Don't abstract all numbers to N and strings to 'S'</td></tr>
<tr><td><code>-d</code>, <code>--debug</code></td><td>Debug</td></tr>
<tr><td><code>-g PATTERN</code></td><td>Grep: only consider statements that include this string</td></tr>
<tr><td><code>--help</code></td><td>Display help</td></tr>
<tr><td><code>-h HOSTNAME</code></td><td>Hostname of db server for *-slow.log filename (can be wildcard), default is '*', i.e. match all</td></tr>
<tr><td><code>-i NAME</code></td><td>Name of server instance (if using mysql.server startup script)</td></tr>
<tr><td><code>-l</code></td><td>Don't subtract lock time from total time</td></tr>
<tr><td><code>-n NUM</code></td><td>Abstract numbers with at least <em>NUM</em> digits within names</td></tr>
<tr><td><code>-r</code></td><td>Reverse the sort order (largest last instead of first)</td></tr>
<tr><td><code>-s ORDER</code></td><td>What to sort by (aa, ae, al, ar, at, a, c, e, l, r, t). <code>at</code> is default. <br><code>aa</code> average rows affected <br><code>ae</code> aggregated number of rows examined <br><code>al</code> average lock time <br><code>ar</code> average rows sent <br><code>at</code> average query time <br><code>a</code> rows affected <br><code>c</code> count <br><code>e</code> rows examined <br><code>l</code> lock time <br><code>r</code> rows sent <br><code>t</code> query time</td></tr>
<tr><td><code>-t NUM</code></td><td>Just show the top <em>NUM</em> queries.</td></tr>
<tr><td><code>-v</code>, <code>--verbose</code></td><td>Verbose mode.</td></tr>
</tbody></table>