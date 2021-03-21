# Building Cassandra Storage Engine for Packaging

THIS PAGE IS OBSOLETE, it describes how to build a branch of MariaDB-5.5 with Cassandra SE. Cassandra SE is a part of [MariaDB 10.0](/kb/en/what-is-mariadb-100/), which uses different approach to building.

These are instructions on how exactly we build Cassandra SE packages.

## Getting into build environment

See How_to_access_buildbot_VMs page on the internal wiki. The build VM to use is

```sql
ezvm  precise-amd64-build
```

Get into the VM and continue to next section.

## Set up Thrift

```sql
mkdir build
cd build
wget https://dist.apache.org/repos/dist/release/thrift/0.8.0/thrift-0.8.0.tar.gz

sudo apt-get install bzr
sudo apt-get install flex

tar zxvf thrift-0.8.0.tar.gz 
cd thrift-0.8.0/

./configure --prefix=/home/buildbot/build/thrift-inst --without-qt4 --without-c_glib --without-csharp --without-java --without-erlang --without-python --without-perl --without-php --without-php_extension --without-ruby --without-haskell --without-go --without-d
make
make install

# free some space
make clean
cd .. 
```

## Get the bzr checkout

- Create another SSH connection to terrier, run the script suggested by motd.
- Press (C-a C-c) to create another window
- Copy the base bazaar repository into the VM:

```sql
scp /home/psergey/5.5-cassandra-base.tgz runvm:
```

Then, get back to the window with VM, and run in VM:

```sql
tar zxvf ../5.5-cassandra-base.tgz
rm -rf ../5.5-cassandra-base.tgz
cd 5.5-cassandra/
bzr pull lp:~maria-captains/maria/5.5-cassandra
```

## Compile

```sql
export LIBS="-lthrift"
export LDFLAGS=-L/home/buildbot/build/thrift-inst/lib

mkdir mkdist
cd mkdist
cmake ..
make dist
```

```sql
basename mariadb-*.tar.gz .tar.gz > ../distdirname.txt

cp mariadb-5.5.25.tar.gz ../
cd ..
tar zxf "mariadb-5.5.25.tar.gz"
mv "mariadb-5.5.25" build
cd build
mkdir mkbin
cd mkbin
cmake -DBUILD_CONFIG=mysql_release ..
make -j4 package
```

This should end with:

```sql
CPack: - package: /home/buildbot/build/5.5-cassandra/build/mkbin/mariadb-5.5.25-linux-x86_64.tar.gz generated.
```

Free up some disk space:

```sql
rm -fr ../../mkdist/
```

```sql
mv mariadb-5.5.25-linux-x86_64.tar.gz ../..
cd ../..
rm -rf build
```

## Patch the tarball to include Thrift

```sql
mkdir fix-package
cd fix-package
tar zxvf ../mariadb-5.5.25-linux-x86_64.tar.gz 
```

Verify that mysqld was built with Cassandra SE:

```sql
ldd mariadb-5.5.25-linux-x86_64/bin/mysqld
```

This should point to libthrift-0.8.0.so.

```sql
cp /home/buildbot/build/thrift-inst/lib/libthrift* mariadb-5.5.25-linux-x86_64/lib/
tar czf mariadb-5.5.25-linux-x86_64.tar.gz mariadb-5.5.25-linux-x86_64/
cp mariadb-5.5.25-linux-x86_64.tar.gz ..
```

## Copy the data out of VM

In the second window (the one that's on terrier, but not in VM), run:

```sql
mkdir build-cassandra
cd build-cassandra
scp runvm:/home/buildbot/build/5.5-cassandra/mariadb-5.5.25.tar.gz .
scp runvm:/home/buildbot/build/5.5-cassandra/mariadb-5.5.25-linux-x86_64.tar.gz .
```

## References

1 [http://buildbot.askmonty.org/buildbot/builders/kvm-tarbake-jaunty-x86/builds/2578](http://buildbot.askmonty.org/buildbot/builders/kvm-tarbake-jaunty-x86/builds/2578)
2 [http://buildbot.askmonty.org/buildbot/builders/kvm-bintar-hardy-amd64/builds/1907](http://buildbot.askmonty.org/buildbot/builders/kvm-bintar-hardy-amd64/builds/1907)