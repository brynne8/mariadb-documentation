# Building MariaDB From Source Using musl-based GNU/Linux

## Instructions on compiling MariaDB on musl-based operating systems (Alpine)

The instructions on this page will help you compile [MariaDB](/replication/optimization-and-tuning/query-optimizations/guiduuid-performance/mariadb/) from source.
Links to more complete instructions for specific platforms can be found on the
[source](/kb/en/source/) page.

- First, [get a copy of the MariaDB source](/kb/en/getting-the-mariadb-source-code/).
- Next, [prepare your system to be able to compile the source](/kb/en/build-environment-setup-for-linux/).

## Using cmake

[MariaDB 10.1](/kb/en/what-is-mariadb-101/) and above is compiled using <em>cmake</em>. You can configure your
build simply by running <em>cmake</em> using special option, i.e.

```sql
cmake . -DWITHOUT_TOKUDB=1
```

To build and install MariaDB after running <em>cmake</em> use

```sql
make
sudo make install
```

Note that building with MariaDB this way will disable tokuDB, till tokuDB becomes fully supported on musl.