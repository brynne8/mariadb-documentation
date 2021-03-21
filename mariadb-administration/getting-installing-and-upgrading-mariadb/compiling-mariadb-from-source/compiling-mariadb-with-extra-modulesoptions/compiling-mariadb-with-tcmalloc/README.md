# Compiling MariaDB with TCMalloc

[TCMalloc](http://goog-perftools.sourceforge.net/doc/tcmalloc.html) is a malloc replacement library optimized for multi-threaded usage. It also features a built-in heap debugger and profiler.

To build [MariaDB 5.5](/kb/en/what-is-mariadb-55/) with `TCMalloc`, you need to use the following command

```sql
cmake -DCMAKE_EXE_LINKER_FLAGS='-ltcmalloc'  -DWITH_SAFEMALLOC=OFF
```

Many other malloc replacement libraries (as well as heap debuggers and profilers) can be used with MariaDB in a similar fashion.

You can also start a standard MariaDB server with `TCmalloc` with:

```sql
/usr/sbin/mysqld_safe --malloc-lib=tcmalloc
```

### See Also

- [Debugging a running server on Linux](/kb/en/debugging-a-running-server-on-linux/)