# Creating a Debian Repository

Below are instructions for creating your own Debian repository. The instructions are based on 
[http://www.debian.org/doc/manuals/repository-howto/repository-howto.en.html](http://www.debian.org/doc/manuals/repository-howto/repository-howto.en.html)

```sql
REPO_DIR={pick some location}
mkdir $REPO_DIR
mkdir $REPO_DIR/binary
mkdir $REPO_DIR/source
cp *.deb *.ddeb $REPO_DIR/binary
cd $REPO_DIR
dpkg-scanpackages binary  /dev/null | gzip -9c > binary/Packages.gz
dpkg-scansources  source  /dev/null | gzip -9c > source/Sources.gz
```

## Using the Debian repository you just created

One needs to add a new file to the <code class="fixed" style="white-space:pre-wrap">/etc/apt/sources.list.d/</code> directory. For instance a new file called <code class="fixed" style="white-space:pre-wrap">mariadb.list</code>

```sql
# sergey's MariaDB repository
#
deb file:///home/psergey/testrepo binary/
deb-src file:///home/psergey/testrepo source/
```

after which one can run

```sql
apt-get update  # Let apt learn about the new repository
apt-get install mariadb-server
```

and collect bugs :-).

"apt-get install" will spray output of scripts and servers all over /var/log. It is also possible to set DEBIAN_SCRIPT_DEBUG=1 to get some (not all) of it to stdout.

## Cleaning up after failed installation

Run

```sql
dpkg --get-selections | grep mariadb
dpkg --get-selections | grep mysql
```

to see what is installed, and then

```sql
 dpkg --purge <packages>
```

until the former produces empty output. Note: after some failures, /etc/mysql and /var/lib/mysql are not cleaned and still need to be removed manually.