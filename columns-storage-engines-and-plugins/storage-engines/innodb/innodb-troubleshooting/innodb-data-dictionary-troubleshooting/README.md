# InnoDB Data Dictionary Troubleshooting

## Can't Open File

If InnoDB returns something like the following error:

```sql
ERROR 1016: Can't open file: 'x.ibd'. (errno: 1)
```

it may be that an orphan `.frm` file exists. Something like the following may also appear in the [error log](/mariadb-administration/server-monitoring-logs/error-log/):

```sql
InnoDB: Cannot find table test/x from the internal data dictionary
InnoDB: of InnoDB though the .frm file for the table exists. Maybe you
InnoDB: have deleted and recreated InnoDB data files but have forgotten
InnoDB: to delete the corresponding .frm files of InnoDB tables?
```

If this is the case, as the text describes, delete the orphan `.frm` file on the filesystem.

## Removing Orphan Intermediate Tables

An orphan intermediate table may prevent you from dropping the tablespace even if it is otherwise empty, and generally takes up unnecessary space.

It may come about if MariaDB exits in the middle of an [ALTER TABLE ... ALGORITHM=INPLACE](/kb/en/alter-table/#algorithm) operation. They will be listed in the [INFORMATION_SCHEMA.INNODB_SYS_TABLES](/kb/en/information-schema-innodb_sys_tables-table/) table, and always start with an `#sql-ib` prefix. The accompanying `.frm` file also begins with `#sql-`, but has a different name.

To identify orphan tables, run:

```sql
SELECT * FROM INFORMATION_SCHEMA.INNODB_SYS_TABLES WHERE NAME LIKE '%#sql%';
```

When [innodb_file_per_table](/kb/en/xtradbinnodb-server-system-variables/#innodb_file_per_table) is set, the `#sql-*.ibd` file will also be visible in the database directory.

To remove an orphan intermediate table:

- Rename the `#sql-*.frm` file (in the database directory) to match the base name of the orphan intermediate table, for example:

```sql
mv #sql-36ab_2.frm #sql-ib87-856498050.frm
```

- Drop the table, for example:

```sql
DROP TABLE `#mysql50##sql-ib87-856498050`;
```

## See Also

- [InnoDB Troubleshooting Overview](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-troubleshooting/innodb-troubleshooting-overview/)