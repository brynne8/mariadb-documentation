# Multi master replication and deadlock detection in Galera with Mariadbs

Hi,

We have multi master Galera replication setup where three master mariadbs are continuously updated. 
1. Because of which occasionally a transaction encounters deadlock. 
2. The JDBC queries slow down their executions if we allow continuous table updates.

Add to the mix an event that purges the tables and we noticed a deletion of more than 200000 entries held the lock for more than 15 seconds.

How can we rectify this problem?
 Is there a way to improve DB performance in replication setup?
We also used non replicated setup and the portal does not encounter any JDBC connection errors.

Your insights are much appreciated.

Thanks