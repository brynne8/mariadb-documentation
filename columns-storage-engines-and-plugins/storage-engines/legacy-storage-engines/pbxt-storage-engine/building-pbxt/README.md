# Building PBXT

As of [MariaDB 5.5](/kb/en/what-is-mariadb-55/) PBXT is not built by default any longer.

The commands to use for building the [MariaDB 5.5 source](/mariadb-administration/getting-installing-and-upgrading-mariadb/compiling-mariadb-from-source) with PBXT:

```sql
cmake -DWITH_PBXT_STORAGE_ENGINE=1 .
make
make install
```

It should be noted that MariaDB still ships the engine, it just does not build it by default.