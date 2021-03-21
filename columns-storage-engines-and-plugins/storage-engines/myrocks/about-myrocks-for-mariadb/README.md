# About MyRocks for MariaDB

# About MyRocks for MariaDB

MyRocks is an open source storage engine that was originally developed by Facebook.

MyRocks has been extended by the MariaDB engineering team to be a pluggable storage engine that you use in your MariaDB solutions. It works seamlessly with MariaDB features. This openness in the storage layer allows you to use the right storage engine to optimize your usage requirements, which provides optimum performance. Community contributions are one of MariaDB’s greatest advantages over other databases. Under the lead of our developer Sergey Petrunia, MyRocks in MariaDB is constantly being merged with upstream MyRocks from Facebook.<br>     
See more at: [https://mariadb.com/resources/blog/facebook-myrocks-mariadb#sthash.ZlEr7kNq.dpuf](https://mariadb.com/resources/blog/facebook-myrocks-mariadb#sthash.ZlEr7kNq.dpuf)

<img src="/kb/en/about-myrocks-for-mariadb/+image/mariaDBstorageEngineOpt" title="storage engine options" height="150" alt="storage engine options" width="900">

MyRocks, typically, gives greater performance for web scale type applications. It can be an ideal storage engine solution when you have workloads that require greater compression and IO efficiency. It uses a Log Structured Merge (LSM) architecture, which has advantages over B-Tree algorithms, to provide efficient data ingestion, like read-free replication slaves, or fast bulk data loading.
MyRocks distinguishing features include:

- compaction filter
- merge operator
- backup
- column families
- bulk loading
- persistent cache<br>

For more MyRocks features see: [https://github.com/facebook/rocksdb/wiki/Features-Not-in-LevelDB](https://github.com/facebook/rocksdb/wiki/Features-Not-in-LevelDB)

## Benefits

On production workloads, MyRocks was tested to prove that it provides:

#### Greater Space Efficiency

- 2x more compression<br> 
MyRocks has 2x better compression compared to compressed InnoDB, 3-4x better compression compared to uncompressed InnoDB, meaning you use less space.

#### Greater Writing Efficiency

- 2x lower write rates to storage<br>
MyRocks has a 10x less write amplification compared to InnoDB, giving you better endurance of flash storage and improving overall throughput.

#### Faster Data Loading

- faster database loads<br>
MyRocks writes data directly onto the bottommost level, which avoids all compaction overheads when you enable faster data loading for a session.

#### Faster Replication

- No random reads for updating secondary keys, except for unique indexes. The Read-Free Replication option does away with random reads when updating primary keys, regardless of uniqueness, with a row-based binary logging format.

[http://myrocks.io](http://myrocks.io)
[https://mariadb.com/resources/blog/facebook-myrocks-mariadb](https://mariadb.com/resources/blog/facebook-myrocks-mariadb)

## Requirements and Limitations

- MyRocks is included from [MariaDB 10.2.5](/kb/en/mariadb-1025-release-notes/).
- MyRocks is available in the MariaDB Server packages for Linux and Windows.
- Maria DB optimistic parallel replication may not be supported.
- MyRocks is not available for 32-bit platforms
- [Galera Cluster](/kb/en/galera/) is tightly integrated into InnoDB storage engine (it also supports Percona's XtraDB which is a modified version of InnoDB). Galera Cluster does not work with any other storage engines, including MyRocks (or TokuDB for example).

MyRocks builds are available on platforms that support a sufficiently modern compiler, for example:

- Ubuntu Trusty, Xenial, (amd64 and ppc64el)
- Ubuntu Yakkety (amd64)

- Debian Jessie, stable (amd64, ppc64el)
- Debian Stretch, Sid (testing and unstable) (amd64)

- CentOS/RHEL 7 (amd64)
- Centos/RHEL 7.3 (amd64)
- Fedora 24 and 25 (amd64)
- OpenSUSE 42 (amd64)

- Windows 64 (zip and MSI)