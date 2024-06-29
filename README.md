[![Contributions](https://img.shields.io/badge/contributions-welcome-orange?style=flat-square)](https://github.com/camponogaraviera/full-stack-roadmap/pulls)
[![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg)](https://github.com/camponogaraviera/full-stack-roadmap/graphs/commit-activity)

<div align='center'>
  <h1> Full-Stack Roadmap </h1>
  <h2> JavaScript + Data Structures & Algorithms + System Design & Infrastructure + Mock Design Interviews + AWS </h2>
</div>

# Foreword

Every system is built on an infrastructure with a particular design (architecture). This repository covers programming fundamentals, system design principles, and service layers required for deployment.

# Table of Contents

## 1. Programming Languages

- [Introduction to JavaScript ES5-ES13](https://github.com/camponogaraviera/javascript)

## 2. [Data Structures & Algorithms](https://github.com/camponogaraviera/ds-and-algo)

- [Theory](https://github.com/camponogaraviera/ds-and-algo/tree/dev/theory)
- [Quiz](https://github.com/camponogaraviera/ds-and-algo/blob/dev/Quiz/data_structures.md)
- Implementations:
  - [JavaScript](https://github.com/camponogaraviera/ds-and-algo/blob/dev/javascript/README.md)
  - [Python](https://github.com/camponogaraviera/ds-and-algo/blob/dev/python/README.md)
    
## 3. System Design & Infrastructure

### 3.1 [URL & DNS](system_design_and_infrastructure/01_url_dns.md)
  - URL
  - DNS
  - Registering a DNS with AWS
    
### 3.2 [Servers](system_design_and_infrastructure/02_servers.md)
  - Stateful vs Stateless 
  - Cold standby
  - Warm standby
  - Hot standby 

### 3.3 [Over-Provisioning](system_design_and_infrastructure/03_over_provisioning.md)

### 3.4 [Vertical Scaling](system_design_and_infrastructure/04_vertical_scaling.md)

### 3.5 [Horizontal Scaling a.k.a Sharding](system_design_and_infrastructure/05_horizontal_scaling.md)
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
    
### 3.6 [Prefetching and Caching](system_design_and_infrastructure/06_prefetching_and_caching.md)
  - Prefetching
  - Caching
    - Expiration policy
    - The cold-start problem in massive-scale systems
    - Eviction Cache Policies
    - Caching Technologies
    
### 3.7 [CDN](system_design_and_infrastructure/07_cdn.md)

### 3.8 [The Hot Spot (Celebrity) Problem](system_design_and_infrastructure/08_celebrity.md)
    
### 3.9 Database

- [OLAP vs OLTP](system_design_and_infrastructure/database/00_olap_vs_oltp.md)
- [Relational Database](system_design_and_infrastructure/database/01_relational_db.md)
- [Non-Relational (NoSQL) Databases](system_design_and_infrastructure/database/02_non_relational_db.md)
- [ACID Properties](system_design_and_infrastructure/database/03_ACID.md)
- [CAP Theorem a.k.a Brewer's Theorem](system_design_and_infrastructure/database/04_CAP_theorem.md)
- [Data Normalization vs Denormalization](system_design_and_infrastructure/database/05_norm_denorm.md)
- Technologies
  - [DynamoDB](system_design_and_infrastructure/database/06_technologies/DynamoDB.md)
  - [MongoDB](system_design_and_infrastructure/database/06_technologies/MongoDB.md)
  - [MongoDB VS DynamoDB](system_design_and_infrastructure/database/06_technologies/mongoVSdynamo.md)
  - [Neo4j](system_design_and_infrastructure/database/06_technologies/Neo4j.md)

### 3.10 Frontend

- [ReactJS](system_design_and_infrastructure/frontend/reactjs.md)
- [React Native](https://github.com/camponogaraviera/react-native)
- [Apollo vs Redux](system_design_and_infrastructure/frontend/apollo_vs_redux.md)

### 3.11 Backend

- [Node.js](system_design_and_infrastructure/backend/nodejs.md)
- [RESTful API](system_design_and_infrastructure/backend/restfull_api.md)
- [GraphQL](system_design_and_infrastructure/backend/grahql.md)
- [gRPC](system_design_and_infrastructure/backend/gRPC.md)
- [CORS](system_design_and_infrastructure/backend/cors.md)

## 4 [AWS](https://github.com/camponogaraviera/aws)

## 5. Mock Design Interviews (DynamoDB + AWS)

- [Design a Restaurant Reservation System](mock_design/rest_reservation.md)
- [Design a YouTube-like On-Demand Video Streaming Service](mock_design/youtube.md)
- [Design a Search Engine like Google](mock_design/google_search_engine.md)
- [Design a Big Data Pipeline for Batch and Real-Time Analytics](mock_design/data_pipeline.md)
