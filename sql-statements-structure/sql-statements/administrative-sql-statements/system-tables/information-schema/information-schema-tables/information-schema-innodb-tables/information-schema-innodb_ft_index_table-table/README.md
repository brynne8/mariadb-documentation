# Information Schema INNODB_FT_INDEX_TABLE Table

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

The `INNODB_FT_INDEX_TABLE` table was added in [MariaDB 10.0](/kb/en/what-is-mariadb-100/).

The [Information Schema](/kb/en/information_schema/) `INNODB_FT_INDEX_TABLE` table contains information about InnoDB [fulltext indexes](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes/). To avoid re-organizing the fulltext index each time a change is made, which would be very expensive, new changes are stored separately and only integrated when an [OPTIMIZE TABLE](/replication/optimization-and-tuning/optimizing-tables/optimize-table/) is run. See the [INNODB_FT_INDEX_CACHE](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-innodb-tables/information-schema-innodb_ft_index_cache-table/) table.

The `SUPER` [privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) is required to view the table, and it also requires the [innodb_ft_aux_table](/kb/en/innodb-system-variables/#innodb_ft_aux_table) system variable to be set.

It has the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>WORD</code></td><td>Word from the text of a column with a fulltext index. Words can appear multiple times in the table, once per <code>DOC_ID</code> and <code>POSITION</code> combination.</td></tr>
<tr><td><code>FIRST_DOC_ID</code></td><td>First document ID where this word appears in the index.</td></tr>
<tr><td><code>LAST_DOC_ID</code></td><td>Last document ID where this word appears in the index.</td></tr>
<tr><td><code>DOC_COUNT</code></td><td>Number of rows containing this word in the index.</td></tr>
<tr><td><code>DOC_ID</code></td><td>Document ID of the newly added row, either an appropriate ID column or an internal InnoDB value.</td></tr>
<tr><td><code>POSITION</code></td><td>Position of this word instance within the <code>DOC_ID</code>, as an offset added to the previous <code>POSITION</code> instance.</td></tr>
</tbody></table>

Note that for `OPTIMIZE TABLE` to process InnoDB fulltext index data, the [innodb_optimize_fulltext_only](innodb-server-system-variables#innodb_optimize_fulltext_only) system variable needs to be set to `1`. When this is done, and an `OPTIMIZE TABLE` statement run, the [INNODB_FT_INDEX_CACHE](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-innodb-tables/information-schema-innodb_ft_index_cache-table/) table will be emptied, and the `INNODB_FT_INDEX_TABLE` table will be updated.

## Examples

```sql
SELECT * FROM INNODB_FT_INDEX_TABLE;
Empty set (0.00 sec)

SET GLOBAL innodb_optimize_fulltext_only =1;

OPTIMIZE TABLE test.ft_innodb;
+----------------+----------+----------+----------+
| Table          | Op       | Msg_type | Msg_text |
+----------------+----------+----------+----------+
| test.ft_innodb | optimize | status   | OK       |
+----------------+----------+----------+----------+

SELECT * FROM INNODB_FT_INDEX_TABLE;
+------------+--------------+-------------+-----------+--------+----------+
| WORD       | FIRST_DOC_ID | LAST_DOC_ID | DOC_COUNT | DOC_ID | POSITION |
+------------+--------------+-------------+-----------+--------+----------+
| and        |            4 |           5 |         2 |      4 |        0 |
| and        |            4 |           5 |         2 |      5 |        0 |
| arrived    |            4 |           4 |         1 |      4 |       20 |
| ate        |            1 |           5 |         2 |      1 |        4 |
| ate        |            1 |           5 |         2 |      5 |        8 |
| everybody  |            1 |           1 |         1 |      1 |        8 |
| goldilocks |            4 |           4 |         1 |      4 |        9 |
| hungry     |            3 |           3 |         1 |      3 |        8 |
| pear       |            5 |           5 |         1 |      5 |       14 |
| she        |            5 |           5 |         1 |      5 |        4 |
| then       |            4 |           4 |         1 |      4 |        4 |
| wicked     |            2 |           2 |         1 |      2 |        4 |
| witch      |            2 |           2 |         1 |      2 |       11 |
+------------+--------------+-------------+-----------+--------+----------+
```