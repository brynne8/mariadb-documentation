# Creating the MariaDB Source Tarball

How to create a source tar.gz file.

First [Setup your build environment](/kb/en/Linux_Build_Environment_Setup/).

Then use automake/configure/make to generate the tar file:

```sql
BUILD/autorun.sh
./configure --with-plugin-xtradb
make dist
```

This creates a source file with a name like <code class="highlight fixed" style="white-space:pre-wrap">mysql-5.3.2-MariaDB-beta.tar.gz</code>

See also [the generic build instructions](/mariadb-administration/getting-installing-and-upgrading-mariadb/compiling-mariadb-from-source/generic-build-instructions).