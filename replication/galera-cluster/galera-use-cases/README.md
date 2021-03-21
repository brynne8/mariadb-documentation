# Galera Use Cases

Here are some common use cases for Galera replication:

<table><tbody><tr><th>Read Master</th><td>Traditional MariaDB master-slave topology, but with Galera all "slave" nodes are capable masters at all times - it is just the application that treats them as slaves. Galera replication can guarantee zero slave lag for such installations and, due to parallel slave applying, much better throughput for the cluster.</td></tr>
<tr><th>WAN Clustering</th><td>Synchronous replication works fine over the WAN network. There will be a delay, which is proportional to the network round trip time (RTT), but it only affects the commit operation.</td></tr>
<tr><th>Disaster Recover</th><td>Disaster recovery is a sub-class of WAN replication. Here one data center is passive and only receives replication events, but does not process any client transactions. Such a remote data center will be up to date at all times and no data loss can happen. During recovery, the spare site is just nominated as primary and application can continue as normal with a minimal fail over delay.</td></tr>
<tr><th>Latency Eraser</th><td>With WAN replication topology, cluster nodes can be located close to clients. Therefore all read &amp; write operations will be super fast with the local node connection. The RTT related delay will be experienced only at commit time, and even then it can be generally accepted by end user, usually the kill-joy for end user experiences is the slow browsing response time, and read operations are as fast as they possibly can be.</td></tr>
</tbody></table>

## See Also

- [What is MariaDB Galera Cluster?](/replication/galera-cluster/what-is-mariadb-galera-cluster)
- [About Galera Replication](/replication/galera-cluster/about-galera-replication)
- [Codership: Using Galera Cluster](http://codership.com/content/using-galera-cluster)
- [Getting Started with MariaDB/Galera Cluster](/replication/galera-cluster/getting-started-with-mariadb-galera-cluster)