# Information Schema INNODB_FT_DEFAULT_STOPWORD Table

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

The `INNODB_FT_DEFAULT_STOPWORD` table was added in [MariaDB 10.0](/kb/en/what-is-mariadb-100/) and MySQL 5.6

The [Information Schema](/kb/en/information_schema/) `INNODB_FT_DEFAULT_STOPWORD` table contains a list of default [stopwords](/kb/en/stopwords/) used when creating an InnoDB [fulltext index](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes/).

The `PROCESS` [privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) is required to view the table.

It has the following column:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>VALUE</code></td><td>Default <code><a href="/kb/en/stopwords/">stopword</a></code> for an InnoDB <a href="/kb/en/full-text-indexes/">fulltext index</a>. Setting either the <a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_ft_server_stopword_table">innodb_ft_server_stopword_table</a> or the <a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_ft_user_stopword_table">innodb_ft_user_stopword_table</a> system variable will override this.</td></tr>
</tbody></table>

## Example

```sql
SELECT * FROM information_schema.INNODB_FT_DEFAULT_STOPWORD\G
*************************** 1. row ***************************
value: a
*************************** 2. row ***************************
value: about
*************************** 3. row ***************************
value: an
*************************** 4. row ***************************
value: are
...
*************************** 36. row ***************************
value: www
```