# Extended Show

The following [SHOW](/sql-statements-structure/sql-statements/administrative-sql-statements/show) statements can be extended by using a `WHERE` clause and a `LIKE` clause to refine the results:

- [SHOW CHARACTER SET](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-character-set)
- [SHOW COLLATION](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-collation)
- [SHOW COLUMNS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-columns)
- [SHOW DATABASES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-databases)
- [SHOW FUNCTION STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-function-status)
- [SHOW INDEX](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-index)<code>
</code>
- [SHOW OPEN TABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-open-tables)
- [SHOW PACKAGE STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-package-status)
- [SHOW PACKAGE BODY STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-package-body-status)
- [SHOW INDEX](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-index)
- [SHOW PROCEDURE STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-procedure-status)
- [SHOW STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-status)
- [SHOW TABLE STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-table-status)
- [SHOW TABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-tables)
- [SHOW TRIGGERS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-triggers)
- [SHOW VARIABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-variables)

As with a regular [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select), the `WHERE` clause can be used for the specific columns returned, and the [LIKE](/built-in-functions/string-functions/like) clause with the regular wildcards.

## Examples

```sql
SHOW TABLES;
+----------------------+
| Tables_in_test       |
+----------------------+
| animal_count         |
| animals              |
| are_the_mooses_loose |
| aria_test2           |
| t1                   |
| view1                |
+----------------------+
```

Showing the tables beginning with <em>a</em> only.

```sql
SHOW TABLES WHERE Tables_in_test LIKE 'a%';
+----------------------+
| Tables_in_test       |
+----------------------+
| animal_count         |
| animals              |
| are_the_mooses_loose |
| aria_test2           |
+----------------------+
```

Variables whose name starts with <em>aria</em> and with a valued of greater than 8192:

```sql
SHOW VARIABLES WHERE Variable_name LIKE 'aria%' AND Value >8192;
+------------------------------+---------------------+
| Variable_name                | Value               |
+------------------------------+---------------------+
| aria_checkpoint_log_activity | 1048576             |
| aria_log_file_size           | 1073741824          |
| aria_max_sort_file_size      | 9223372036853727232 |
| aria_pagecache_buffer_size   | 134217728           |
| aria_sort_buffer_size        | 134217728           |
+------------------------------+---------------------+
```

Shortcut, just returning variables whose name begins with <em>aria</em>.

```sql
SHOW VARIABLES LIKE 'aria%';
+------------------------------------------+---------------------+
| Variable_name                            | Value               |
+------------------------------------------+---------------------+
| aria_block_size                          | 8192                |
| aria_checkpoint_interval                 | 30                  |
| aria_checkpoint_log_activity             | 1048576             |
| aria_force_start_after_recovery_failures | 0                   |
| aria_group_commit                        | none                |
| aria_group_commit_interval               | 0                   |
| aria_log_file_size                       | 1073741824          |
| aria_log_purge_type                      | immediate           |
| aria_max_sort_file_size                  | 9223372036853727232 |
| aria_page_checksum                       | ON                  |
| aria_pagecache_age_threshold             | 300                 |
| aria_pagecache_buffer_size               | 134217728           |
| aria_pagecache_division_limit            | 100                 |
| aria_recover                             | NORMAL              |
| aria_repair_threads                      | 1                   |
| aria_sort_buffer_size                    | 134217728           |
| aria_stats_method                        | nulls_unequal       |
| aria_sync_log_dir                        | NEWFILE             |
| aria_used_for_temp_tables                | ON                  |
+------------------------------------------+---------------------+
```