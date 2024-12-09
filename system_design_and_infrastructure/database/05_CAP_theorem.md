<div align='center'>
  <h1>CAP Theorem a.k.a Brewer's Theorem</h1>
</div>

# Table of Contents

- About
  - Availability 
  - Consistency
  - Partition-Tolerance 
- Tradeoffs
- Database Classification

# About

The CAP theorem states that a distributed database system can only satisfy two out of the three following properties: Availability, Consistency, and Partition-Tolerance.

Note1: for example, when using DynamoDB, even though database replications are still possible, in the presence of a communication breakdown, the system will sacrifice Availability to maintain Consistency and Partition-Tolerance.

Note2: nodes can be physical machines (such as servers) or virtual instances (such as Docker containers or virtual machines) running on physical machines.

## Availability 

Every request (read or write) is guaranteed to receive a (non-error) response, even if one or more nodes fail, but the response might not be up-to-date. It avoids single points of failure by `replicating the data across multiple nodes`.

## Consistency 

While not related to ACID principles, consistency means that data is consistent (the same) across all nodes, and can be retrieved fast once written. To achieve that, `data is replicated across nodes`.

- Strong consistency: when the replication is immediate, every read operation returns the most recent write (is up-to-date) or an error.
- Eventual consistency: when the replication is not immediate, the returned data might be stale.

## Partition-Tolerance 

The overall system continues to operate even if there are network partitions (communication breakdown or information loss) between nodes.

# Tradeoffs

- Consistency and Partition-Tolerance (CP): guarantees that the data is consistent, but may sacrifice availability (some requests may fail).

- Availability and Partition-Tolerance (AP): guarantees that the system remains available (responds to requests) but may serve stale or inconsistent data.

- Consistency and Availability (CA): guarantees that every request receives the most recent write (consistency) and that every request receives a response (availability), however, this is only possible in the absence of network partitions.

# Database Classification

- MySQL, PostgreSQL, and MariaDB: prioritizes `Consistency` and `Availability` over Partition-Tolerance. This database is used when availability and consistency are more important than scalability. Use cases: designing the database of a small bank.

- Apache HBase, MongoDB, and DynamoDB: prioritizes `Consistency` and `Partition-Tolerance`. Availability is circumvented by using hot standbys.

- Cassandra and CouchDB: prioritizes `Availability` and `Partition-Tolerance`. Unlike master-slave architectures, Cassandra ensures availability by replicating data across multiple nodes via a peer-to-peer ring structure where all nodes are equal, i.e., any host/node can serve as the primary host (there is no master node). This decentralized architecture ensures there is no single point of failure.

Note1: in massive systems, `Partition-Tolerance` is not negotiable!

Note2: single-master design prioritizes `Consistency` and `Partition-Tolerance`.
