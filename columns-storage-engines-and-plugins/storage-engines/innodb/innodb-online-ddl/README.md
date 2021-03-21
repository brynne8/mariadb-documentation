# InnoDB Online DDL

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

InnoDB tables support online DDL, which permits concurrent DML and uses optimizations to avoid unnecessary table copying.

- [InnoDB Online DDL Overview](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-online-ddl/innodb-online-ddl-overview/) — All about online DDL operations with InnoDB.
- [InnoDB Online DDL Operations with the INPLACE Alter Algorithm](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-online-ddl/innodb-online-ddl-operations-with-the-inplace-alter-algorithm/) — These DDL operations can be done in-place with InnoDB.
- [InnoDB Online DDL Operations with the NOCOPY Alter Algorithm](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-online-ddl/innodb-online-ddl-operations-with-the-nocopy-alter-algorithm/) — These DDL operations can be done without copying the table with InnoDB.
- [InnoDB Online DDL Operations with the INSTANT Alter Algorithm](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-online-ddl/innodb-online-ddl-operations-with-the-instant-alter-algorithm/) — These DDL operations can be done instantly with InnoDB.
- [Instant ADD COLUMN for InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-online-ddl/instant-add-column-for-innodb/) — Instantly add a new column to a table