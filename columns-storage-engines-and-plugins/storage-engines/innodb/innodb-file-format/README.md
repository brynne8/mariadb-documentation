# InnoDB File Format

Prior to [MariaDB 10.3](/kb/en/what-is-mariadb-103/), the [XtraDB/InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) storage engine supports two different file formats.

## Setting a Table's File Format

In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and before, the default file format for InnoDB tables can be chosen by setting the [innodb_file_format](/kb/en/xtradbinnodb-server-system-variables/#innodb_file_format).

In [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/) and before, the default file format is`Antelope`. In [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/) and later, the default file format is `Barracuda` and `Antelope` is deprecated.

A table's tablespace is tagged with the lowest InnoDB file format that supports the table's [row format](/kb/en/xtradbinnodb-storage-formats/). So, even if the `Barracuda` file format is enabled, tables that use the `COMPACT` or `REDUNDANT` row formats will be tagged with the `Antelope` file format in the [information_schema.INNODB_SYS_TABLES](/kb/en/information-schema-innodb_sys_tables-table/) table.

## Supported File Formats

The [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) storage engine supports two different file formats:

- `Antelope`
- `Barracuda`

### Antelope

In [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/) and before, the default file format is `Antelope`. In [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/) and later, the `Antelope` file format is deprecated.

`Antelope` is the original InnoDB file format. It supports the `COMPACT` and `REDUNDANT` row formats, but not the `DYNAMIC` or `COMPRESSED` row formats.

### Barracuda

In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and before, the `Barracuda` file format is only supported if the [innodb_file_per_table](/kb/en/xtradbinnodb-server-system-variables/#innodb_file_per_table) system variable is set to `ON`. In [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/) and later, the default file format is `Barracuda` and `Antelope` is deprecated.

`Barracuda` is a newer InnoDB file format. It supports the `COMPACT`, `REDUNDANT`, `DYNAMIC` and `COMPRESSED` row formats. Tables with large BLOB or TEXT columns in particular could benefit from the dynamic row format.

### Future Formats

InnoDB might use new file formats in the future. Each format will have an identifier from 0 to 25, and a name. The names have already been decided, and are animal names listed in an alphabetical order: Antelope, Barracuda, Cheetah, Dragon, Elk, Fox, Gazelle, Hornet, Impala, Jaguar, Kangaroo, Leopard, Moose, Nautilus, Ocelot, Porpoise, Quail, Rabbit, Shark, Tiger, Urchin, Viper, Whale, Xenops, Yak and Zebra.

## Checking a Table's File Format.

In [MariaDB 10.0](/kb/en/what-is-mariadb-100/) and later, the [information_schema.INNODB_SYS_TABLES](/kb/en/information-schema-innodb_sys_tables-table/) table can be queried to see the file format used by a table.

A table's tablespace is tagged with the lowest InnoDB file format that supports the table's [row format](/kb/en/xtradbinnodb-storage-formats/). So, even if the `Barracuda` file format is enabled, tables that use the `COMPACT` or `REDUNDANT` row formats will be tagged with the `Antelope` file format in the [information_schema.INNODB_SYS_TABLES](/kb/en/information-schema-innodb_sys_tables-table/) table.

## Compatibility

Each tablespace is tagged with the id of the most recent file format used by one of its tables. All versions of XtraDB/InnoDB can read tables that use an older file format. However, it can not read from more recent formats. For this reason, each time XtraDB/InnoDB opens a table it checks the tablespace's format, and returns an error if a newer format is used.

This check can be skipped via the [innodb_file_format_check](/kb/en/xtradbinnodb-server-system-variables/#innodb_file_format_check) variable. Beware that, is XtraDB/InnoDB tries to repair a table in an unknown format, the table will be corrupted! This happens on restart if innodb_file_format_check is disabled and the server crashed, or it was closed with fast shutdown.

To downgrade a table from the Barracuda format to Antelope, the table's `ROW_FORMAT` can be set to a value supported by Antelope, via an [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table) statement. This recreates the indexes.

The Antelope format can be used to make sure that tables work on MariaDB and MySQL servers which are older than 5.5.

## See Also

- [InnoDB Storage Formats](/kb/en/innodb-storage-formats/)