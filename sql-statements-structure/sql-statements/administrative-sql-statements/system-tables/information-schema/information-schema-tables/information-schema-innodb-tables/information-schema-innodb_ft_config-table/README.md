# Information Schema INNODB_FT_CONFIG Table

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

The `INNODB_FT_CONFIG` table was added in [MariaDB 10.0](/kb/en/what-is-mariadb-100/) and MySQL 5.6

The [Information Schema](/kb/en/information_schema/) `INNODB_FT_CONFIG` table contains InnoDB [fulltext index](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes/) metadata.

The `SUPER` [privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) is required to view the table, and it also requires the [innodb_ft_aux_table](/kb/en/xtradbinnodb-server-system-variables/#innodb_ft_aux_table) system variable to be set.

It has the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>KEY</code></td><td>Metadata item name.</td></tr>
<tr><td><code>VALUE</code></td><td>Associated value.</td></tr>
</tbody></table>

## Example

```sql
SELECT * FROM INNODB_FT_CONFIG;
+---------------------------+-------+
| KEY                       | VALUE |
+---------------------------+-------+
| optimize_checkpoint_limit | 180   |
| synced_doc_id             | 6     |
| last_optimized_word       |       |
| deleted_doc_count         | 0     |
| total_word_count          |       |
| optimize_start_time       |       |
| optimize_end_time         |       |
| stopword_table_name       |       |
| use_stopword              | 1     |
| table_state               | 0     |
+---------------------------+-------+
```