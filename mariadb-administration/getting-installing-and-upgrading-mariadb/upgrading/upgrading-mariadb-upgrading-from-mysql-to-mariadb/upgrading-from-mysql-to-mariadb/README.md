# Upgrading from MySQL to MariaDB

For [all practical purposes](/kb/en/mariadb-vs-mysql-compatibility/), you can view MariaDB as an upgrade of MySQL:

- Before upgrading, please [check if there are any known incompatibilities](/kb/en/mariadb-vs-mysql-compatibility/) between your MySQL release and the MariaDB release you want to move to.
- If you are using MySQL 8.0 or above, you have to use  [mysqldump](/clients-utilities/backup-restore-and-import-clients/mysqldump/) to move your database to MariaDB.
- For upgrading from very old MySQL versions, see [Upgrading to MariaDB from MySQL 5.0 (or older version)](/kb/en/upgrading-to-mariadb-from-mysql-50-or-older-version/).
- Within the same base version (for example MySQL 5.5 -&gt; [MariaDB 5.5](/kb/en/what-is-mariadb-55/), MySQL 5.6 -&gt; [MariaDB 10.0](/kb/en/what-is-mariadb-100/) and MySQL 5.7 -&gt; [MariaDB 10.2](/kb/en/what-is-mariadb-102/)) you can in most cases just uninstall MySQL and install MariaDB and you are good to go. There is no need to dump and restore databases. As with any upgrade, we recommend making a backup of your data beforehand.
- You should run [mysql_upgrade](/sql-statements-structure/sql-statements/table-statements/mysql_upgrade/) (just as you would with MySQL) to finish the upgrade. This is needed to ensure that your mysql privilege and event tables are updated with the new fields MariaDB uses. Note that if you use a MariaDB package, `mysql_upgrade` is usually run automatically.
- All your old clients and connectors (PHP, Perl, Python, Java, etc.) will work
  unchanged (no need to recompile). This works because MariaDB and MySQL use
  the same client protocol and the client libraries are binary compatible. You can also use your old MySQL connector packages with MariaDB if you want.

## [Upgrading on Windows](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-mariadb-on-windows/)

On Windows, you should not uninstall MySQL and install MariaDB, this would not work, the existing database will not be found.

Thus On Windows, just install MariaDB and use the upgrade wizard which is part of installer package and is launched by MSI installer. Or, in case you prefer command line, use `mysql_upgrade_service &lt;service_name&gt;` on the command line.

## Upgrading my.cnf

All the options in your original MySQL [`my.cnf` file](/mariadb-administration/getting-installing-and-upgrading-mariadb/mysqld-configuration-files-and-groups/) should work fine for MariaDB.

However as MariaDB has more features than MySQL, there is a few things that you should consider changing in your `my.cnf` file.

- MariaDB uses by default the [Aria storage engine](/columns-storage-engines-and-plugins/storage-engines/aria/aria-storage-engine/) for internal temporary files instead of MyISAM. If you have a lot of temporary files, you should add and set <a undefined>aria-pagecache-buffer-size</a> to the same value as you have for <a undefined>key-buffer-size</a>.
- If you don't use MyISAM tables, you can set <a undefined>key-buffer-size</a> to a very low value, like 64K.
- If using [MariaDB 10.1](/kb/en/what-is-mariadb-101/) or earlier, and your applications often connect and disconnect to MariaDB, you should set up <a undefined>thread-cache-size</a> to the number of concurrent queries threads you are typically running. This is important in MariaDB as we are using the [jemalloc](http://www.canonware.com/jemalloc/) memory allocator.  [jemalloc](http://www.canonware.com/jemalloc/) usually has better performance when running many threads compared to other memory allocators, except if you create and destroy a lot of threads, in which case it will spend a lot of resources trying to manage thread specific storage.  Having a thread cache will fix this problem.
- If you have a LOT of connections (&gt; 100) that mostly run short running queries, you should consider using the [thread pool](/kb/en/threadpool-in-55/). For example using : <a undefined>thread_handling=pool-of-threads</a> and <a undefined>thread_pool_size=128</a> could give a notable performance boost in this case. Where the `thread_pool_size` should be about `2 * number of cores on your machine`.

## Other Things to Think About

- Views with definition `ALGORITHM=MERGE` or `ALGORITHM=TEMPTABLE` got accidentally swapped between MariaDB and MySQL. You have to re-create views created with either of these definitions (see [MDEV-6916](https://jira.mariadb.org/browse/MDEV-6916)).
- MariaDB has LGPL versions of the [C connector](/kb/en/client-library-for-c/) and [Java Client](/kb/en/mariadb-java-client/). If you are shipping an application that supports MariaDB or MySQL, you should consider using these!
- You should consider trying out the [MyRocks storage engine](/columns-storage-engines-and-plugins/storage-engines/myrocks/) or some of the other [new storage engines](/kb/en/mariadb-storage-engines/) that MariaDB provides.

## See Also

- MariaDB has a lot of [new features](/kb/en/mariadb-vs-mysql-features/) that you should know about.
- [MariaDB versus MySQL - Compatibility](/kb/en/mariadb-vs-mysql-compatibility/)
- [Migrating to MariaDB](/migrating-to-mariadb/)
- You can find general upgrading informations on the [MariaDB installation page](/mariadb-administration/getting-installing-and-upgrading-mariadb/).
- There is a [Screencast for upgrading MySQL to MariaDB](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-mariadb-upgrading-from-mysql-to-mariadb/screencast-for-upgrading-mysql-to-mariadb/).
- [Upgrading to MariaDB in Debian 9](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-mariadb-upgrading-from-mysql-to-mariadb/moving-from-mysql-to-mariadb-in-debian-9/)