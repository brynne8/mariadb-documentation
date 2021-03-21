# mysql_convert_table_format

##### MariaDB starting with [10.4.6](/kb/en/mariadb-1046-release-notes/)

From [MariaDB 10.4.6](/kb/en/mariadb-1046-release-notes/), `mariadb-convert-table-format` is a symlink to `mysql_convert_table_format`.

##### MariaDB starting with [10.5.2](/kb/en/mariadb-1052-release-notes/)

From [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), `mariadb-convert-table-format` is the name of the tool, with `mysql_convert_table_format` a symlink .

## Usage

```sql
mysql_convert_table_format [options] db_name
```

## Description

mysql_convert_table_format converts the tables in a database to use a particular storage engine ([MyISAM](/kb/en/myisam/) by default).

mysql_convert_table_format is written in Perl and requires that the DBI and DBD::mysql Perl modules be installed

Invoke mysql_convert_table_format like this:

```sql
shell> mysql_convert_table_format [options]db_name
```

The `db_name` argument indicates the database containing the tables to be converted.

## Options

`mysql_convert_table_format` supports the options described in the following list:

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td>--help</td><td>Display a help message and exit.</td></tr>
<tr><td>--force</td><td>Continue even if errors occur.</td></tr>
<tr><td>--host=host_name</td><td>Connect to the MariaDB server on the given host.</td></tr>
<tr><td>--password=password</td><td>The password to use when connecting to the server. Note that the password value is not optional for this option, unlike for other client programs. Specifying the password on the command-line is generally considered insecure.</td></tr>
<tr><td>--port=port_num</td><td>The TCP/IP port number to use for the connection.</td></tr>
<tr><td>--socket=path</td><td>For connections to localhost, the Unix socket file to use.</td></tr>
<tr><td>--type=engine_name</td><td>Specify the storage engine that the tables should be converted to use. The default is <a href="/kb/en/myisam/">MyISAM</a> if this option is not given.</td></tr>
<tr><td>--user=user_name</td><td>The MariaDB user name to use when connecting to the server.</td></tr>
<tr><td>--verbose</td><td>Verbose mode. Print more information about what the program does.</td></tr>
<tr><td>--version</td><td>Display version information and exit.</td></tr>
</tbody></table>