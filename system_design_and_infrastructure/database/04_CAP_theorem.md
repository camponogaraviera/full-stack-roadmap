<div align='center'>
  <h1>CAP Theorem a.k.a Brewer's Theorem</h1>
</div>

# Table of Contents

- About
- Availability 
- Consistency
- Partition-Tolerance 
- Database Classification

# About

The CAP theorem states that a distributed database system can only satisfy two out of the three following properties: Availability, Consistency, and Partition-Tolerance.

Note: practically, many databases satisfy all the below three properties, however, technically one has to be sacrificed. For example, when using DynamoDB, even though database replications are still possible, in the presence of a communication breakdown, the system will sacrifice Availability to maintain Consistency and Partition-Tolerance.

# Availability 

A server is still accessible even if a node fails. It avoids single points of failure by replicating the data across multiple servers.

# Consistency 

While not related to ACID principles, it means that data is consistent (the same) across all servers, and can be retrieved fast once written, i.e., any read operation returns the most recent write.

# Partition-Tolerance 

The system continues to operate even if there are network partitions (communication breakdown) between nodes.

# Database Classification

- MySQL: prioritizes `Availability` and `Consistency`, and gives up Partition-Tolerance. This database is used when availability and consistency are more important than scalability. Use cases: designing the database of a small bank.

- Apache HBase, MongoDB, or DynamoDB: prioritizes `Consistency` and `Partition-Tolerance`. Availability is circumvented by using hot standbys.

- Cassandra: prioritizes `Availability` and `Partition-Tolerance`. Cassandra ensures availability via a ring structure where any host/node can serve as the primary host (there is no master node). Partition-Tolerance means that one can keep adding nodes to the ring. Consistency is discarded because data needs to propagate (be replicated) through the entire ring since there is no single master.

Note 1: in massive systems, `Partition-Tolerance` is not negotiable!

Note 2: single-master design prioritizes `Consistency` and `Partition-Tolerance`.
