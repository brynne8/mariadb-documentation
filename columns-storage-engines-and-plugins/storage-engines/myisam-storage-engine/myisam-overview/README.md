# MyISAM Overview

The MyISAM storage engine was the default storage engine from MySQL 3.23 until it was replaced by [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) in MariaDB and MySQL 5.5. Historically, MyISAM is a replacement for the older ISAM engine, removed in MySQL 4.1.

It's a light, non-transactional engine with great performance, is easy to copy between systems and has a small data footprint.

You're encouraged to rather use the [Aria](/columns-storage-engines-and-plugins/storage-engines/aria) storage engine for new applications, which has even better performance in most cases and the goal of being crash-safe.

A MyISAM table is stored in three files on disk. There's a table definition file with an extension of `.frm`, a data file with the extension `.MYD`, and an index file with the extension `.MYI`.

## MyISAM features

- Does not support [transactions](/sql-statements-structure/sql-statements/transactions).
- Does not support foreign keys.
- Supports [FULLTEXT indexes](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes).
- Supports [GIS](/kb/en/gis-functionality/) data types.
- Storage limit of 256TB.
- Maximum of 64 indexes per table.
- Maximum of 32 columns per index.
- Maximum index length of 1000 bytes.
- Limit of (2<sup>32</sup>)<sup>2</sup> (1.844E+19) rows per table.
- Supports large files up to 63-bits in length where the underlying system supports this.
- All data is stored with the low byte first, so all files will still work if copied to other systems or other machines.
- The data file and the index file can be placed on different devices to improve speed.
- Supports table locking, not row locking.
- Supports a key buffer that is [segmented](/replication/optimization-and-tuning/system-variables/segmented-key-cache) in MariaDB.
- Supports [concurrent inserts](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/concurrent-inserts).
- Supports fixed length, dynamic and compressed formats - see [MyISAM Storage Formats](/columns-storage-engines-and-plugins/storage-engines/myisam-storage-engine/myisam-storage-formats).
- Numeric index values are stored with the high byte first, which enables more efficient index compression.
- Data values are stored with the low byte first, making it mostly machine and operating system independent. The only exceptions are if a machine doesn't use two's-complement signed integers and the IEEE floating-point format.
- Can be copied between databases or systems with normal system tools, as long as the files are not open on either system. Use [FLUSH_TABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush) to ensure files are not in use.
- There are a number of tools for working with MyISAM tables. These include:
<ul><li>[mysqlcheck](/sql-statements-structure/sql-statements/table-statements/mysqlcheck) for checking or repairing
</li><li>[myisamchk](/clients-utilities/myisam-clients-and-utilities/myisamchk) for checking or repairing
</li><li>[myisampack](/clients-utilities/myisam-clients-and-utilities/myisampack) for compressing
</li></ul>
- It is possible to build a [MERGE](/columns-storage-engines-and-plugins/storage-engines/merge) table on the top of one or more MyISAM tables.