# InnoDB Data Scrubbing

##### MariaDB starting with [10.1.3](/kb/en/mariadb-1013-release-notes/)

Data scrubbing was introduced in [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/).

Note that most of the background and redo log scrubbing code has been removed in [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/). See [MDEV-15528](https://jira.mariadb.org/browse/MDEV-15528) and [MDEV-21870](https://jira.mariadb.org/browse/MDEV-21870).

Sometimes there is a requirement that when some data is deleted, it is really gone. This might be the case when one stores user's personal information or some other sensitive data. Normally though, when a row is deleted, the space is only marked as free on the page. It may eventually be overwritten, but there is no guarantee when that will happen. A copy of the deleted rows may also be present in the log files.

[MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/) introduced support for [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) data scrubbing. Background threads periodically scan tablespaces and logs and remove all data that should be deleted. The number of background threads for tablespace scans is set by [innodb-encryption-threads](/kb/en/xtradbinnodb-server-system-variables/#innodb_encryption_threads). Log scrubbing happens in a separate thread.

To configure scrubbing one can use the following variables:

<table><tbody><tr><td><a href="/kb/en/innodb-system-variables/#innodb_background_scrub_data_check_interval">innodb-background-scrub-data-check-interval</a></td><td>Seconds</td><td>Check at this intervall if tablespaces needs scrubbing. Deprecated and ignored from <a href="/kb/en/mariadb-1052-release-notes/">MariaDB 10.5.2</a>.</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_background_scrub_data_compressed">innodb-background-scrub-data-compressed</a></td><td>Boolean</td><td>Enable scrubbing of compressed data by background threads. Deprecated and ignored from <a href="/kb/en/mariadb-1052-release-notes/">MariaDB 10.5.2</a>.</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_background_scrub_data_interval">innodb-background-scrub-data-interval</a></td><td>Seconds</td><td>Scrub spaces that were last scrubbed longer than this many seconds ago. Deprecated and ignored from <a href="/kb/en/mariadb-1052-release-notes/">MariaDB 10.5.2</a>.</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_background_scrub_data_uncompressed">innodb-background-scrub-data-uncompressed</a></td><td>Boolean</td><td>Enable scrubbing of uncompressed data by background threads. Deprecated and ignored from <a href="/kb/en/mariadb-1052-release-notes/">MariaDB 10.5.2</a>.</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_immediate_scrub_data_uncompressed">innodb-immediate-scrub-data-uncompressed</a></td><td>Boolean</td><td>Enable scrubbing of uncompressed data</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_scrub_log">innodb-scrub-log</a></td><td>Boolean</td><td>Enable redo log scrubbing. Deprecated and ignored from <a href="/kb/en/mariadb-1052-release-notes/">MariaDB 10.5.2</a>.</td></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_scrub_log_speed">innodb-scrub-log-speed</a></td><td>Bytes/sec</td><td>Redo log scrubbing speed in bytes/sec. Deprecated and ignored from <a href="/kb/en/mariadb-1052-release-notes/">MariaDB 10.5.2</a>.</td></tr>
</tbody></table>

Redo log scrubbing did not fully work as intended, and was deprecated and ignored in [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/) ([MDEV-21870](https://jira.mariadb.org/browse/MDEV-21870)). If old log contents should be kept secret, then enabling [innodb_encrypt_log](/kb/en/innodb-system-variables/#innodb_encrypt_log) or setting a smaller [innodb_log_file_size](/kb/en/innodb-system-variables/#innodb_log_file_size) could help.

The [Information Schema INNODB_TABLESPACES_SCRUBBING table](/kb/en/information-schema-innodb_tablespaces_scrubbing-table/) contains scrubbing information.

## Thanks

- Scrubbing was donated to the MariaDB project by Google.