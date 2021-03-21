# InnoDB Row Formats

- [InnoDB Row Formats Overview](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-row-formats-overview/) — InnoDB's row formats are REDUNDANT, COMPACT, DYNAMIC, and COMPRESSED.
- [InnoDB REDUNDANT Row Format](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-redundant-row-format/) — The REDUNDANT row format is the original non-compacted row format.
- [InnoDB COMPACT Row Format](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-compact-row-format/) — Similar to the REDUNDANT row format, but stores data in a more compact manner.
- [InnoDB DYNAMIC Row Format](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-dynamic-row-format/) — Similar to the COMPACT row format, but can store even more data on overflow pages.
- [InnoDB COMPRESSED Row Format](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/innodb-compressed-row-format/) — Similar to the COMPACT row format, but can store even more data on overflow pages.
- [Troubleshooting Row Size Too Large Errors with InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-row-formats/troubleshooting-row-size-too-large-errors-with-innodb/) — Fixing "Row size too large (> 8126). Changing some columns to TEXT or BLOB may help."