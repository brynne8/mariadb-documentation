# mysql_waitpid

##### MariaDB starting with [10.4.6](/kb/en/mariadb-1046-release-notes/)

From [MariaDB 10.4.6](/kb/en/mariadb-1046-release-notes/), `mariadb-waitpid` is a symlink to `mysql_waitpid`.

##### MariaDB starting with [10.5.2](/kb/en/mariadb-1052-release-notes/)

From [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), `mysql_waitpid` is the symlink, and `mariadb-waitpid` the binary name.

`mysql_pid` is a utility for terminating processes. It runs on Unix-like systems, making use of the `kill()` system call.

## Usage

```sql
mysql_waitpid [options] pid time
```

## Description

`mysql_pid` sends signal 0 to the process <em>pid</em> and waits up to <em>time</em> seconds for the process to terminate. <em>pid</em> and <em>time</em> must be positive integers.

Returns 0 if the process terminates in time, or does not exist, and 1 otherwise.

Signal 1 is used if the kill() system call cannot handle signal 0

## Options

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>-?</code>, <code>--help</code></td><td>Display help and exit</td></tr>
<tr><td><code>-I</code>, <code>--help</code></td><td>Synonym for -?</td></tr>
<tr><td><code>-v</code>, <code>--verbose</code></td><td>Be more verbose. Give a warning, if kill can't handle signal 0</td></tr>
<tr><td><code>-V</code>, <code>--version</code></td><td>Print version information and exit</td></tr>
</tbody></table>