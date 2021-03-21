# mysql_fix_extensions

##### MariaDB starting with [10.4.6](/kb/en/mariadb-1046-release-notes/)

From [MariaDB 10.4.6](/kb/en/mariadb-1046-release-notes/), `mariadb-fix-extensions` is a symlink to `mysql_fix_extensions`.

##### MariaDB starting with [10.5.2](/kb/en/mariadb-1052-release-notes/)

From [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), `mysql_fix_extensions` is the symlink, and `mariadb-fix-extensions` the binary name.

`mysql_fix_extensions` converts the extensions for [MyISAM](/kb/en/myisam/) (or ISAM) table files to their canonical forms.

It looks for files with extensions matching any lettercase variant of `.frm`, `.myd`, `.myi`, `.isd`, and `.ism` and renames them to have extensions of `.frm`, `.MYD`, `.MYI`, `.ISD`, and `.ISM`, respectively. This can be useful after transferring the files from a system with case-insensitive file names (such as Windows) to a system with case-sensitive file names.

Invoke mysql_fix_extensions as follows, where data_dir is the path name to the MariaDB data directory.

```sql
mysql_fix_extensions data_dir
```