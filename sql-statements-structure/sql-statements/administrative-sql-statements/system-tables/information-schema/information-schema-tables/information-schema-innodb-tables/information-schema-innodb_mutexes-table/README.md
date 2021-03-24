# Information Schema INNODB_MUTEXES Table

##### MariaDB starting with [10.1.3](/kb/en/mariadb-1013-release-notes/)

The [Information Schema](/kb/en/information_schema/) `INNODB_MUTEXES` table was added in [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)

The `INNODB_MUTEXES` table monitors mutex and rw locks waits. It has the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>NAME</code></td><td>Name of the lock, as it appears in the source code.</td></tr>
<tr><td><code>CREATE_FILE</code></td><td>File name of the mutex implementation.</td></tr>
<tr><td><code>CREATE_LINE</code></td><td>Line number of the mutex implementation.</td></tr>
<tr><td><code>OS_WAITS</code></td><td>How many times the mutex occurred.</td></tr>
</tbody></table>

The `CREATE_FILE` and `CREATE_LINE` columns depend on the InnoDB/XtraDB version.

Note that since [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/), the table has only been providing information about
rw_lock_t, not any mutexes. From [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/) until [MariaDB 10.2.32](/kb/en/mariadb-10232-release-notes/), [MariaDB 10.3.23](/kb/en/mariadb-10323-release-notes/), [MariaDB 10.4.13](/kb/en/mariadb-10413-release-notes/) and [MariaDB 10.5.1](/kb/en/mariadb-1051-release-notes/), the `NAME` column was not populated ([MDEV-21636](https://jira.mariadb.org/browse/MDEV-21636)).

The [SHOW ENGINE INNODB STATUS](/kb/en/show-engine/#show-engine-innodb-mutex) statement provides similar information.

## Examples

```sql
SELECT * FROM INNODB_MUTEXES;
+------------------------------+---------------------+-------------+----------+
| NAME                         | CREATE_FILE         | CREATE_LINE | OS_WAITS |
+------------------------------+---------------------+-------------+----------+
| &dict_sys->mutex             | dict0dict.cc        |         989 |        2 |
| &buf_pool->flush_state_mutex | buf0buf.cc          |        1388 |        1 |
| &log_sys->checkpoint_lock    | log0log.cc          |        1014 |        2 |
| &block->lock                 | combined buf0buf.cc |        1120 |        1 |
+------------------------------+---------------------+-------------+----------+
```