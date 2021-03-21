# Creating the MariaDB Binary Tarball

How to generate binary tar.gz files.

- [Setup your build environment](/kb/en/Linux_Build_Environment_Setup/).
- [Build binaries](http://kb.askmonty.org/en/generic-build-instructions) with your preferred options/plugins.

##### MariaDB until [5.3](/kb/en/what-is-mariadb-53/)

- Use <code class="highlight fixed" style="white-space:pre-wrap">make_binary_distribution</code> to generate a binary tar file:

```sql
cd mariadb-source
./scripts/make_binary_distribution
```

This creates a source file with a name like <code class="highlight fixed" style="white-space:pre-wrap">mariadb-5.3.2-MariaDB-beta-linux-x86_64.tar.gz</code> in your current directory.

The other option is to use the bakery scripts. In this case you don't have to compile MariaDB source first.

```sql
cd $PACKAGING_WORK
bakery/preheat.sh
cd bakery_{number}
bakery/tarbake51.sh last:1 $MARIA_WORK
bakery/autobake51-bintar.sh mariadb-{version_num}-maria-beta-ourdelta{number}.tar.gz
```

##### MariaDB starting with [5.5](/kb/en/what-is-mariadb-55/)

If the binaries are already built, you can generate a binary tarball with

```sql
make package
```