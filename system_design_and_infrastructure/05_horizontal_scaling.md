<div align='center'>
  <h1> Horizontal Scaling </h1>
</div>

# Table of Contents

- About
- Components of Horizontal Scaling
  - Load Balancer
  - Routing Algorithms
    - Round robin
    - Weighted round-robin
  - Consistent Hashing
  - Database Replication
  - Multi-master Replication
  - Bidirectional and Circular Replication

# About

In horizontal scaling a.k.a sharding, scaling in (up) is the practice of increasing the number of servers, i.e., computer instances. This practice increases the number of machines (server nodes) rather than its read and write capacity units, and every server should be stateless. Horizontal scaling is difficult to implement with SQL databases, but possible in modern MySQL using Vitess (a database clustering system created at YouTube).

- Pros: a distributed/sharding system, which relies on multiple machines, increases fault tolerance, and reduces latency by allowing multi-region deployments in regions closer to the user's location.
- Cons: are more expensive than vertical scaling in the short run. Requires a distributed database.

# Components of Horizontal Scaling 

## Load Balancer

A load balancer redirects/offloads/distributes incoming traffic (requests) across a fleet of servers to avoid a stack overflow. Private IP addresses are then assigned to the load balancer for connecting clients to available servers that will handle requests. Clients only have access to the load balancer IP which is public, not to the private ones.

## Routing Algorithms

- `Round robin load balancing algorithm`: presumes that all servers have the same RCU and WCU read and write capacity units. Requests are then routed to servers in order: the 1st request gets routed to server 1, the 2nd request to server 2, and so on.

- `Weighted round robin load balancing algorithm`: allows traffic to be distributed across servers according to their provisioned Read Capacity Unity (RCU) and Write Capacity Unity (WCU). Servers with more read and write capacity can handle more requests.

## Consistent Hashing

In distributed systems, data needs to be distributed among shards. Typically, this is achieved using a hashing function that assigns sharding keys a.k.a partition keys, such as user_id and IPs used to retrieve and modify data. However, shards can become overloaded with time (shard exhaustion), and in addition, adding (scaling up) and removing (scaling down) shards causes uneven data distribution. To circumvent this issue, consistent hashing is employed for data resharding (redistribution).

## Database Replication

In this approach, a master/slave relationship is used to ensure data Availability (see CAP theorem). 

- Master: is the original database used to handle write operations.

- Slave: is a copy of the master database used to handle read operations.

When a master goes down, a slave database is ready to take its place.

## Multi-master Replication

## Bidirectional and Circular Replication
