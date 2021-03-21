# ColumnStore Minimum Hardware Specification

The following table outlines the minimum recommended production server specifications which can be followed for both on premise and cloud deployments:

# Single server

<table><tbody><tr><th>Item</th><th>Development Environment</th><th>Production Environment</th></tr>
<tr><td>Physical Server</td><td>8 core Intel / AMD, 32GB Memory</td><td>32 core Intel / AMD, 64 GB Memory</td></tr>
<tr><td>Storage</td><td>Local disk</td><td>Local disk</td></tr>
</tbody></table>

# Multi server

<table><tbody><tr><th>Item</th><th>Development Environment</th><th>Production Environment</th></tr>
<tr><td>UM Physical Server</td><td>8 core Intel / AMD, 32GB Memory</td><td>32 core Intel / AMD, 64 GB Memory</td></tr>
<tr><td>PM Physical Server</td><td>8 core Intel / AMD, 16GB Memory</td><td>16 core Intel / AMD, 32GB Memory</td></tr>
</tbody></table>

<table><tbody><tr><th>Item</th><th>Description</th></tr>
<tr><td>Storage</td><td>Local disk on each PM can be appropriate for systems that can tolerate some down time in the event of server failure. To leverage the automated fail-over capabilities, a networked storage layer such as SAN for on premise or EBS in AWS is a better choice. A distributed filesystem such as the open source GlusterFS will also allow for node fail-over. The storage system must support files being opened with the O_DIRECT flag as the system utilizes this to optimize block caching and avoid the OS redundantly caching the same blocks.</td></tr>
<tr><td>Network Interconnect</td><td>In a multi server deployment data will be passed around via TCP/IP networking. At least a 1G network is recommended.</td></tr>
</tbody></table>

# Details

These are minimum recommendations and in general the system will perform better with more hardware:

- More CPU cores and servers will improve query processing response time.
- More memory will allow the system to cache more data blocks in memory. We have users running system with anywhere from 64G RAM to 512 G RAM for UM and 32 to 64 G RAM for PM.
- Faster network will allow data to flow faster between UM and PM nodes.
- SSD's may be used, however the system is optimized towards block streaming which may perform well enough with HDD's for lower cost.
- Where it is an option, it is recommended to use bare metal servers for additional performance since ColumnStore will fully consume CPU cores and memory.
- In general it makes more sense to use a higher core count / higher memory server for single server or 2 server combined deployments.
- In a deployment with multiple UM nodes the system will round robin requests from the mysqld handling the query to any ExeMgr in the cluster for load balancing. A higher bandwidth network such as 10g or 40g will be of benefit for large result set queries.

## AWS instance sizes

For AWS our own internal testing generally uses m4.4xlarge instance types as a cost effective middle ground. The R4.8xlarge has also been tested and performs about twice as fast for about twice the price.