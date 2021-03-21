# SHOW CREATE SEQUENCE

##### MariaDB starting with [10.3.1](/kb/en/mariadb-1031-release-notes/)

Sequences were introduced in [MariaDB 10.3](/kb/en/what-is-mariadb-103/).

## Syntax

```sql
SHOW CREATE SEQUENCE sequence_name;
```

## Description

Shows the [CREATE SEQUENCE](/sql-statements-structure/sequences/create-sequence/) statement that created the given sequence. The statement requires the `SELECT` privilege for the table.

## Example

```sql
CREATE SEQUENCE s1 START WITH 50;
SHOW CREATE SEQUENCE s1\G;
*************************** 1. row ***************************
       Table: s1
Create Table: CREATE SEQUENCE `s1` start with 50 minvalue 1 maxvalue 9223372036854775806 
  increment by 1 cache 1000 nocycle ENGINE=InnoDB
```

## Notes

If you want to see the underlying table structure used for the `SEQUENCE`
you can use [SHOW CREATE TABLE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-table/) on the `SEQUENCE`. You can also use `SELECT` to read the current recorded state of the `SEQUENCE`:

```sql
SHOW CREATE TABLE s1\G
*************************** 1. row ***************************
       Table: s1
Create Table: CREATE TABLE `s1` (
  `next_not_cached_value` bigint(21) NOT NULL,
  `minimum_value` bigint(21) NOT NULL,
  `maximum_value` bigint(21) NOT NULL,
  `start_value` bigint(21) NOT NULL COMMENT 'start value when sequences is created 
     or value if RESTART is used',
  `increment` bigint(21) NOT NULL COMMENT 'increment value',
  `cache_size` bigint(21) unsigned NOT NULL,
  `cycle_option` tinyint(1) unsigned NOT NULL COMMENT '0 if no cycles are allowed, 
     1 if the sequence should begin a new cycle when maximum_value is passed',
  `cycle_count` bigint(21) NOT NULL COMMENT 'How many cycles have been done'
) ENGINE=InnoDB SEQUENCE=1

SELECT * FROM s1\G
*************************** 1. row ***************************
next_not_cached_value: 50
        minimum_value: 1
        maximum_value: 9223372036854775806
          start_value: 50
            increment: 1
           cache_size: 1000
         cycle_option: 0
          cycle_count: 0
```

## See Also

- [CREATE SEQUENCE](/sql-statements-structure/sequences/create-sequence/)
- [ALTER SEQUENCE](/sql-statements-structure/sequences/alter-sequence/)