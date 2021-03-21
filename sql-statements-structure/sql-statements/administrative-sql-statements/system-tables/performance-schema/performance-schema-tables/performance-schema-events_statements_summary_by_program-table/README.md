# Performance Schema events_statements_summary_by_program Table

##### MariaDB starting with [10.5.2](/kb/en/mariadb-1052-release-notes/)

The `events_statements_summary_by_program` table, along with many other new [Performance Schema tables](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/list-of-performance-schema-tables), was added in [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/).

The [Performance Schema](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema) events_statements_summary_by_program table contains the following fields.

```sql
DESC performance_schema.events_statements_summary_by_program;
+-----------------------------+--------------------------------------------------------+------+-----+---------+-------+
| Field                       | Type                                                   | Null | Key | Default | Extra |
+-----------------------------+--------------------------------------------------------+------+-----+---------+-------+
| OBJECT_TYPE                 | enum('EVENT','FUNCTION','PROCEDURE','TABLE','TRIGGER') | YES  |     | NULL    |       |
| OBJECT_SCHEMA               | varchar(64)                                            | NO   |     | NULL    |       |
| OBJECT_NAME                 | varchar(64)                                            | NO   |     | NULL    |       |
| COUNT_STAR                  | bigint(20) unsigned                                    | NO   |     | NULL    |       |
| SUM_TIMER_WAIT              | bigint(20) unsigned                                    | NO   |     | NULL    |       |
| MIN_TIMER_WAIT              | bigint(20) unsigned                                    | NO   |     | NULL    |       |
| AVG_TIMER_WAIT              | bigint(20) unsigned                                    | NO   |     | NULL    |       |
| MAX_TIMER_WAIT              | bigint(20) unsigned                                    | NO   |     | NULL    |       |
| COUNT_STATEMENTS            | bigint(20) unsigned                                    | NO   |     | NULL    |       |
| SUM_STATEMENTS_WAIT         | bigint(20) unsigned                                    | NO   |     | NULL    |       |
| MIN_STATEMENTS_WAIT         | bigint(20) unsigned                                    | NO   |     | NULL    |       |
| AVG_STATEMENTS_WAIT         | bigint(20) unsigned                                    | NO   |     | NULL    |       |
| MAX_STATEMENTS_WAIT         | bigint(20) unsigned                                    | NO   |     | NULL    |       |
| SUM_LOCK_TIME               | bigint(20) unsigned                                    | NO   |     | NULL    |       |
| SUM_ERRORS                  | bigint(20) unsigned                                    | NO   |     | NULL    |       |
| SUM_WARNINGS                | bigint(20) unsigned                                    | NO   |     | NULL    |       |
| SUM_ROWS_AFFECTED           | bigint(20) unsigned                                    | NO   |     | NULL    |       |
| SUM_ROWS_SENT               | bigint(20) unsigned                                    | NO   |     | NULL    |       |
| SUM_ROWS_EXAMINED           | bigint(20) unsigned                                    | NO   |     | NULL    |       |
| SUM_CREATED_TMP_DISK_TABLES | bigint(20) unsigned                                    | NO   |     | NULL    |       |
| SUM_CREATED_TMP_TABLES      | bigint(20) unsigned                                    | NO   |     | NULL    |       |
| SUM_SELECT_FULL_JOIN        | bigint(20) unsigned                                    | NO   |     | NULL    |       |
| SUM_SELECT_FULL_RANGE_JOIN  | bigint(20) unsigned                                    | NO   |     | NULL    |       |
| SUM_SELECT_RANGE            | bigint(20) unsigned                                    | NO   |     | NULL    |       |
| SUM_SELECT_RANGE_CHECK      | bigint(20) unsigned                                    | NO   |     | NULL    |       |
| SUM_SELECT_SCAN             | bigint(20) unsigned                                    | NO   |     | NULL    |       |
| SUM_SORT_MERGE_PASSES       | bigint(20) unsigned                                    | NO   |     | NULL    |       |
| SUM_SORT_RANGE              | bigint(20) unsigned                                    | NO   |     | NULL    |       |
| SUM_SORT_ROWS               | bigint(20) unsigned                                    | NO   |     | NULL    |       |
| SUM_SORT_SCAN               | bigint(20) unsigned                                    | NO   |     | NULL    |       |
| SUM_NO_INDEX_USED           | bigint(20) unsigned                                    | NO   |     | NULL    |       |
| SUM_NO_GOOD_INDEX_USED      | bigint(20) unsigned                                    | NO   |     | NULL    |       |
+-----------------------------+--------------------------------------------------------+------+-----+---------+-------+
```