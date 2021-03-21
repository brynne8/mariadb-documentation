# InnoDB Doublewrite Buffer

The [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/) doublewrite buffer was implemented to recover from half-written pages. This can happen when there's a power failure while InnoDB is writing a page to disk. On reading that page, InnoDB can discover the corruption from the mismatch of the page checksum. However, in order to recover, an intact copy of the page would be needed.

The double write buffer provides such a copy.

Whenever InnoDB flushes a page to disk, it is first written to the double write buffer. Only when the buffer is safely flushed to disk will InnoDB write the page to the final destination. When recovering, InnoDB scans the double write buffer and for each valid page in the buffer checks if the page in the data file is valid too.

## Doublewrite Buffer Settings

##### MariaDB until [10.0](/kb/en/what-is-mariadb-100/)

Before [MariaDB 10.0](/kb/en/what-is-mariadb-100/), the doublewrite buffer can be moved to a different drive, reducing contention on random reads. Use the [innodb_doublewrite_file](/kb/en/innodb-system-variables/#innodb_doublewrite_file) system variable to specify a location. Since the doublewrite buffer is mostly sequential writes, a traditional HDD is a better choice than SSD. This variable was removed in [MariaDB 10.0](/kb/en/what-is-mariadb-100/).

To turn off the doublewrite buffer, set the [innodb_doublewrite](/kb/en/innodb-system-variables/#innodb_doublewrite) system variable to `0`. This is safe on filesystems that write pages atomically - that is, a page write fully succeeds or fails. But with other filesystems, it is not recommended for production systems. An alternative option is atomic writes. See [atomic write support](/mariadb-administration/getting-installing-and-upgrading-mariadb/mariadb-performance-advanced-configurations/atomic-write-support/) for more details.