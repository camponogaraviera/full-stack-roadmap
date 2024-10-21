[![Contributions](https://img.shields.io/badge/contributions-welcome-orange?style=flat-square)](https://github.com/camponogaraviera/full-stack-roadmap/pulls)
[![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg)](https://github.com/camponogaraviera/full-stack-roadmap/graphs/commit-activity)

<div align='center'>
  <h1> Full-Stack Roadmap </h1>
  <h2> JavaScript + Data Structures & Algorithms + System Design + Mock Design Interviews + IaaS (AWS) </h2>
</div>

# Table of Contents

## 1. Programming Languages

- [Introduction to JavaScript ES5-ES13](https://github.com/camponogaraviera/javascript)

## 2. [Data Structures & Algorithms](https://github.com/camponogaraviera/ds-and-algo)

- [Theory](https://github.com/camponogaraviera/ds-and-algo/blob/dev/theory/README.md)
- [Quiz](https://github.com/camponogaraviera/ds-and-algo/blob/dev/Quiz/data_structures.md)
- [Leetcode](https://github.com/camponogaraviera/ds-and-algo/blob/dev/leetcode/README.md)
- Implementations:
  - [JavaScript](https://github.com/camponogaraviera/ds-and-algo/blob/dev/javascript/README.md)
  - [Python](https://github.com/camponogaraviera/ds-and-algo/blob/dev/python/README.md)

## 3. System Design 

### 3.1 [URL & DNS](system_design_and_infrastructure/01_url_dns.md)

- URL
- DNS
- Registering a DNS with AWS

### 3.2 [Servers](system_design_and_infrastructure/02_servers.md)

- Stateful vs Stateless
- Cold standby
- Warm standby
- Hot standby

### 3.3 [Over-Provisioning for Resiliency](system_design_and_infrastructure/03_over_provisioning.md)

### 3.4 [Vertical Scaling](system_design_and_infrastructure/04_vertical_scaling.md)

### 3.5 [Sharding and Horizontal Scaling](system_design_and_infrastructure/05_horizontal_scaling.md)

- Sharding
- Horizontal Scaling
  - Load Balancer
  - Routing Algorithms
    - Round robin
    - Weighted round-robin
    - Least Connections 
    - Weighted Least Connections
    - IP Hash
    - Least Response Time
    - Random
    - Least Bandwidth 
  - Consistent Hashing
  - Database Replication
  - Multi-master Replication
  - Bidirectional and Circular Replication

### 3.6 [Prefetching and Caching](system_design_and_infrastructure/06_prefetching_and_caching.md)

- Prefetching
- Caching
  - Expiration policy
  - The cold-start problem in massive-scale systems
  - Eviction Cache Policies
  - Caching Technologies

### 3.7 [CDN](system_design_and_infrastructure/07_cdn.md)

### 3.8 [The Hot Spot (Celebrity) Problem](system_design_and_infrastructure/08_celebrity.md)

### 3.9 [Monolithic vs Microservices](system_design_and_infrastructure/09_mono_vs_micro.md)

### 3.10 [Patterns for Enabling Data Persistence](system_design_and_infrastructure/10_patterns.md)

- Saga Pattern
  - Serverless Implementation
- Database-Per-Service Pattern
- Shared-Database-Per-Service Pattern
- API Composition Pattern
- CQRS Pattern
- Event Sourcing Pattern

### 3.11 [Software Development Life Cycle (SDLC)](system_design_and_infrastructure/11_sldc.md)

- About
- Waterfall Model
- Iterative Model
- Spiral Model
- Agile Model
- V-Model (Validation and Verification Model)
- Rapid Application Development (RAD) Model
- Big Bang Model
- Incremental Model
- DevOps Model

### 3.12 Database

- [OLAP vs OLTP](system_design_and_infrastructure/database/00_olap_vs_oltp.md)
- [Relational Database](system_design_and_infrastructure/database/01_relational_db.md)
- [Non-Relational (NoSQL) Databases](system_design_and_infrastructure/database/02_non_relational_db.md)
- [ACID and BASE Properties](system_design_and_infrastructure/database/03_ACID_BASE.md)
- [CAP Theorem a.k.a Brewer's Theorem](system_design_and_infrastructure/database/04_CAP_theorem.md)
- [Data Normalization vs Denormalization](system_design_and_infrastructure/database/05_norm_denorm.md)
- [Geo-spatial Indexes](system_design_and_infrastructure/database/06_geo_spatial_indexes.md)
  - Geohashing
  - Quadtrees
- [Database Contention vs Thundering Herd](system_design_and_infrastructure/database/07_contention_vs_thundering.md)
- Technologies
  - [DynamoDB](system_design_and_infrastructure/database/06_technologies/DynamoDB.md)
  - [MongoDB](system_design_and_infrastructure/database/06_technologies/MongoDB.md)
  - [MongoDB VS DynamoDB](system_design_and_infrastructure/database/06_technologies/mongoVSdynamo.md)
  - [Neo4j](system_design_and_infrastructure/database/06_technologies/Neo4j.md)

### 3.13 Frontend

- [ReactJS](system_design_and_infrastructure/frontend/reactjs.md)
- [React Native](https://github.com/camponogaraviera/react-native)
- [Apollo vs Redux](system_design_and_infrastructure/frontend/apollo_vs_redux.md)
- [TypeScript](system_design_and_infrastructure/frontend/typescript.md)
- [Next.js](system_design_and_infrastructure/frontend/nextjs.md)

### 3.14 Backend

- [Node.js vs Deno](system_design_and_infrastructure/backend/nodejs_vs_deno.md)
- [RESTful API](system_design_and_infrastructure/backend/restfull_api.md)
- [GraphQL](system_design_and_infrastructure/backend/grahql.md)
- [gRPC](system_design_and_infrastructure/backend/gRPC.md)
- [CORS](system_design_and_infrastructure/backend/cors.md)
- [WebSockets](system_design_and_infrastructure/backend/web_sockets.md)
- [ORM & ODM](system_design_and_infrastructure/backend/orm_odm.md)
- [SST](system_design_and_infrastructure/backend/sst.md)

## 4 [IaaS (AWS)](https://github.com/camponogaraviera/aws)

## 5. Mock Design Interviews (DynamoDB + AWS)

- [Design a Restaurant Reservation System](mock_design/rest_reservation.md)
- [Design a YouTube-like On-Demand Video Streaming Service](mock_design/youtube.md)
- [Design a Search Engine like Google](mock_design/google_search_engine.md)
- [Design a Big Data Pipeline for Batch and Real-Time Analytics](mock_design/data_pipeline.md)

# Refereces

[1] https://aws.amazon.com/

[2] [Mastering the System Design Interview](https://www.sundog-education.com/course/mastering-the-system-design-interview/)

[3] [Awesome DynamoDB](https://github.com/alexdebrie/awesome-dynamodb)

[4] https://react.dev/

[5] https://reactnative.dev/

[6] https://nodejs.org/en

[7] https://deno.com/

[8] https://graphql.org/

[9] https://www.apollographql.com/docs/

[10] https://grpc.io/

[11] https://socket.io/

[12] https://developer.mozilla.org/en-US/docs
