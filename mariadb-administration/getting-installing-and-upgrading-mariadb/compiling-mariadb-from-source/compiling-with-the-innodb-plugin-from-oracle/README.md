# Compiling with the InnoDB Plugin from Oracle

From [MariaDB 10.2](/kb/en/what-is-mariadb-102/), MariaDB uses InnoDB as the default storage engine. Before [MariaDB 10.2](/kb/en/what-is-mariadb-102/), MariaDB came by default with [XtraDB](/kb/en/xtradb/), an enhanced version of the InnoDB plugin that comes from Oracle.

If you want to use Oracle's InnoDB plugin, then you need to compile MariaDB and
<strong>not</strong> specify <code class="fixed" style="white-space:pre-wrap">--without-plugin-innodb_plugin</code> when
configuring. For example, a simple <code class="fixed" style="white-space:pre-wrap">./configure
</code> without
any options will do.

When the InnoDB plugin is compiled, the innodb_plugin test suite will test the
InnoDB plugin in addition to xtradb:

```sql
./mysql-test-run --suite=innodb_plugin
```

To use the innodb_plugin instead of xtradb you can do (for [MariaDB 5.5](/kb/en/what-is-mariadb-55/)):

```sql
mysqld --ignore-builtin-innodb --plugin-load=innodb=ha_innodb.so \
--plugin_dir=/usr/local/mysql/lib/mysql/plugin
```

## See Also

[http://dev.mysql.com/doc/refman/5.1/en/replacing-builtin-innodb.html](http://dev.mysql.com/doc/refman/5.1/en/replacing-builtin-innodb.html)