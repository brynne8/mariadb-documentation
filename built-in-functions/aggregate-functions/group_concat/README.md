# GROUP_CONCAT

## Syntax

```sql
GROUP_CONCAT(expr)
```

## Description

This function returns a string result with the concatenated non-NULL
values from a group. It returns NULL if there are no non-NULL values.

The maximum returned length in bytes is determined by the [group_concat_max_len](/kb/en/server-system-variables/#group_concat_max_len) server system variable, which defaults to 1M (&gt;= [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/)) or 1K (&lt;= [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/)).

If group_concat_max_len &lt;= 512, the return type is [VARBINARY](/columns-storage-engines-and-plugins/data-types/string-data-types/varbinary/) or [VARCHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/varchar/); otherwise, the return type is [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob/) or [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text/). The choice between binary or non-binary types depends from the input.

The full syntax is as follows:

```sql
GROUP_CONCAT([DISTINCT] expr [,expr ...]
             [ORDER BY {unsigned_integer | col_name | expr}
                 [ASC | DESC] [,col_name ...]]
             [SEPARATOR str_val]
             [LIMIT {[offset,] row_count | row_count OFFSET offset}])
```

`DISTINCT` eliminates duplicate values from the output string.

[ORDER BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/order-by/) determines the order of returned values.

`SEPARATOR` specifies a separator between the values. The default separator is a comma (`,`). It is possible to avoid using a separator by specifying an empty string.

### LIMIT

##### MariaDB starting with [10.3.3](/kb/en/mariadb-1033-release-notes/)

Until [MariaDB 10.3.2](/kb/en/mariadb-1032-release-notes/), it was not possible to use the [LIMIT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/limit/) clause with `GROUP_CONCAT`. This restriction was lifted in [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/).

## Examples

```sql
SELECT student_name,
       GROUP_CONCAT(test_score)
       FROM student
       GROUP BY student_name;
```

Get a readable list of MariaDB users from the [mysql.user](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqluser-table/) table:

```sql
SELECT GROUP_CONCAT(DISTINCT User ORDER BY User SEPARATOR '\n')
   FROM mysql.user;
```

In the former example, `DISTINCT` is used because the same user may occur more than once. The new line (`\n`) used as a `SEPARATOR` makes the results easier to read.

Get a readable list of hosts from which each user can connect:

```sql
SELECT User, GROUP_CONCAT(Host ORDER BY Host SEPARATOR ', ') 
   FROM mysql.user GROUP BY User ORDER BY User;
```

The former example shows the difference between the `GROUP_CONCAT`'s [ORDER BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/order-by/) (which sorts the concatenated hosts), and the `SELECT`'s [ORDER BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/order-by/) (which sorts the rows).

From [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/), [LIMIT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/limit/) can be used with `GROUP_CONCAT`, so, for example, given the following table:

```sql
CREATE TABLE d (dd DATE, cc INT);

INSERT INTO d VALUES ('2017-01-01',1);
INSERT INTO d VALUES ('2017-01-02',2);
INSERT INTO d VALUES ('2017-01-04',3);
```

the following query:

```sql
SELECT SUBSTRING_INDEX(GROUP_CONCAT(CONCAT_WS(":",dd,cc) ORDER BY cc DESC),",",1) FROM d;
+----------------------------------------------------------------------------+
| SUBSTRING_INDEX(GROUP_CONCAT(CONCAT_WS(":",dd,cc) ORDER BY cc DESC),",",1) |
+----------------------------------------------------------------------------+
| 2017-01-04:3                                                               |
+----------------------------------------------------------------------------+
```

can be more simply rewritten as:

```sql
SELECT GROUP_CONCAT(CONCAT_WS(":",dd,cc) ORDER BY cc DESC LIMIT 1) FROM d;
+-------------------------------------------------------------+
| GROUP_CONCAT(CONCAT_WS(":",dd,cc) ORDER BY cc DESC LIMIT 1) |
+-------------------------------------------------------------+
| 2017-01-04:3                                                |
+-------------------------------------------------------------+
```

## See Also

- [CONCAT()](/built-in-functions/string-functions/concat/)
- [CONCAT_WS()](/built-in-functions/string-functions/concat_ws/)
- [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/)
- [ORDER BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/order-by/)