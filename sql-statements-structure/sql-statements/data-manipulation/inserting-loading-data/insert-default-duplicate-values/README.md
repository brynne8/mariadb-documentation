# INSERT - Default &amp; Duplicate Values

## Default Values

If the [SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode/) contains `STRICT_TRANS_TABLES` and you are [inserting](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert/) into a transactional table (like InnoDB), or if the SQL_MODE contains `STRICT_ALL_TABLES`, all `NOT NULL` columns which does not have a `DEFAULT` value (and is not [AUTO_INCREMENT](/columns-storage-engines-and-plugins/data-types/auto_increment/)) must be explicitly referenced in `INSERT` statements. If not, an error like this is produced:

```sql
ERROR 1364 (HY000): Field 'col' doesn't have a default value
```

In all other cases, if a `NOT NULL` column without a `DEFAULT` value is not referenced, an empty value will be inserted (for example, 0 for `INTEGER` columns and '' for `CHAR` columns). See [NULL Values in MariaDB:Inserting](/kb/en/null-values-in-mariadb/#inserting) for examples.

If a `NOT NULL` column having a `DEFAULT` value is not referenced, `NULL` will be inserted.

If a `NULL` column having a `DEFAULT` value is not referenced, its default value will be inserted. It is also possible to explicitly assign the default value using the `DEFAULT` keyword or the [DEFAULT()](/built-in-functions/secondary-functions/information-functions/default/) function.

If the `DEFAULT` keyword is used but the column does not have a `DEFAULT` value, an error like this is produced:

```sql
ERROR 1364 (HY000): Field 'col' doesn't have a default value
```

## Duplicate Values

By default, if you try to insert a duplicate row and there is a `UNIQUE` index, `INSERT` stops and an error like this is produced:

```sql
ERROR 1062 (23000): Duplicate entry 'dup_value' for key 'col'
```

To handle duplicates you can use the [IGNORE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/ignore/) clause, [INSERT ON DUPLICATE KEY UPDATE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-on-duplicate-key-update/) or the [REPLACE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/replace/) statement. Note that the IGNORE and DELAYED options are ignored when you use [ON DUPLICATE KEY UPDATE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-on-duplicate-key-update/).

## See Also

- [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert/)
- [INSERT DELAYED](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-delayed/)
- [INSERT SELECT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-select/)
- [HIGH_PRIORITY and LOW_PRIORITY](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/high_priority-and-low_priority/)
- [Concurrent Inserts](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/concurrent-inserts/)
- [INSERT IGNORE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-ignore/)
- [INSERT ON DUPLICATE KEY UPDATE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-on-duplicate-key-update/)