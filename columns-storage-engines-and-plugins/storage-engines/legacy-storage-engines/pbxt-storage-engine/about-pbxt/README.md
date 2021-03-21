# About PBXT

[PBXT](http://www.primebase.org) was a storage engine that was included in the MariaDB source and binaries by default until [MariaDB 5.3](/kb/en/what-is-mariadb-53/).

Since [MariaDB 5.5](/kb/en/what-is-mariadb-55/), the PBXT storage engine has been disabled by default and must be [explicitly built](/kb/en/how-to-build-pbxt/) to use it. The reason is that PBXT is not actively maintained anymore. It has a few bugs that are not fixed and it's not actively used.

The PBXT versions in various releases are:

- version 1.0.11 in [MariaDB 5.1.47](http://askmonty.org/wiki/MariaDB:Download:MariaDB_5.1.47)
- version 1.0.08d in [MariaDB 5.1.44b](http://askmonty.org/wiki/MariaDB:Download:MariaDB_5.1.44b)

PBXT is a general purpose transactional storage engine. PBXT is fully "ACID" compliant, which means it can be used as an alternative to other MariaDB transactional engines (such as XtraDB or InnoDB).

PBXT features include the following:

- <strong>MVCC Support:</strong> MVCC stands for Multi-version Concurrency Control. MVCC allows reading the database without locking.
- <strong>Fully ACID compliant:</strong> This means that all transactions are: atomic, consistent, isolated and durable.
- <strong>Row-level locking:</strong> When updating, PBXT uses row-level locking. Row-level locking is also used during SELECT FOR UPDATE.
- <strong>Fast Rollback and Recovery:</strong> PBXT uses a specialized method to identify garbage which makes "undo" unnecessary. This make both rollback of transactions and recovery after restart very fast.
- <strong>Deadlock Detection:</strong> PBXT identifies all kinds of deadlocks immediately.
- <strong>Write-once:</strong> PBXT uses a log-based storage which makes it possible to write transactional data directly to the database, without first being writen to the transaction log.
- <strong>Referential Integrity:</strong> PBXT supports [foreign key](/replication/optimization-and-tuning/optimization-and-indexes/foreign-keys/) definitions, including cascaded updates and deletes.
- <strong>BLOB streaming:</strong> In combination with the [BLOB Streaming engine](http://www.blobstreaming.org) PBXT can stream binary and media directly in and out of the database.

PBXT will not take any resources (disk space or CPU processing) until you create your first PBXT table.

## xtstat

The included <code class="fixed" style="white-space:pre-wrap">xtstat</code> program can be used to monitor all internal activity of PBXT. See [xtstat](/clients-utilities/xtstat/) for more information.

## More information

Further documentation for PBXT can be found here: [http://www.primebase.org/documentation](http://www.primebase.org/documentation)