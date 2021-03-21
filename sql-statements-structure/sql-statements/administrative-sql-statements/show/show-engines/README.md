# SHOW ENGINES

## Syntax

```sql
SHOW [STORAGE] ENGINES
```

## Description

<code class="highlight fixed" style="white-space:pre-wrap">SHOW ENGINES</code> displays status information about the server's
storage engines. This is particularly useful for checking whether a storage
engine is supported, or to see what the default engine is. 
<code class="highlight fixed" style="white-space:pre-wrap">SHOW TABLE TYPES</code> is a deprecated synonym.

The [information_schema.ENGINES](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-engines-table/) table provides the same information.

Since storage engines are plugins, different information about them is also shown in the <a undefined>information_schema.PLUGINS</a> table and by the [SHOW PLUGINS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-plugins/) statement.

Note that both MySQL's InnoDB and Percona's XtraDB replacement are labeled as `InnoDB`.  However, if XtraDB is in use, it will be specified in the `COMMENT` field. See [XtraDB and InnoDB](/kb/en/xtradb-and-innodb/). The same applies to [FederatedX](/kb/en/federatedx/).

The output consists of the following columns:

- `Engine` indicates the engine's name.
- `Support` indicates whether the engine is installed, and whether it is the default engine for the current session.
- `Comment` is a brief description.
- `Transactions`, `XA` and `Savepoints` indicate whether [transactions](/sql-statements-structure/sql-statements/transactions/), [XA transactions](/sql-statements-structure/sql-statements/transactions/xa-transactions/) and [transaction savepoints](/sql-statements-structure/sql-statements/transactions/savepoint/) are supported by the engine.

## Examples

```sql
SHOW ENGINES\G
*************************** 1. row ***************************
      Engine: InnoDB
     Support: DEFAULT
     Comment: Supports transactions, row-level locking, and foreign keys
Transactions: YES
          XA: YES
  Savepoints: YES
*************************** 2. row ***************************
      Engine: CSV
     Support: YES
     Comment: CSV storage engine
Transactions: NO
          XA: NO
  Savepoints: NO
*************************** 3. row ***************************
      Engine: MyISAM
     Support: YES
     Comment: MyISAM storage engine
Transactions: NO
          XA: NO
  Savepoints: NO
*************************** 4. row ***************************
      Engine: BLACKHOLE
     Support: YES
     Comment: /dev/null storage engine (anything you write to it disappears)
Transactions: NO
          XA: NO
  Savepoints: NO
*************************** 5. row ***************************
      Engine: FEDERATED
     Support: YES
     Comment: FederatedX pluggable storage engine
Transactions: YES
          XA: NO
  Savepoints: YES
*************************** 6. row ***************************
      Engine: MRG_MyISAM
     Support: YES
     Comment: Collection of identical MyISAM tables
Transactions: NO
          XA: NO
  Savepoints: NO
*************************** 7. row ***************************
      Engine: ARCHIVE
     Support: YES
     Comment: Archive storage engine
Transactions: NO
          XA: NO
  Savepoints: NO
*************************** 8. row ***************************
      Engine: MEMORY
     Support: YES
     Comment: Hash based, stored in memory, useful for temporary tables
Transactions: NO
          XA: NO
  Savepoints: NO
*************************** 9. row ***************************
      Engine: PERFORMANCE_SCHEMA
     Support: YES
     Comment: Performance Schema
Transactions: NO
          XA: NO
  Savepoints: NO
*************************** 10. row ***************************
      Engine: Aria
     Support: YES
     Comment: Crash-safe tables with MyISAM heritage
Transactions: NO
          XA: NO
  Savepoints: NO
10 rows in set (0.00 sec)
```