<div align='center'>
  <h1>Design a Search Engine like Google</h1>
</div>

# Functional Requirements

The user should be able to:

- Search for webpages using phrases or keywords.

# Non-Functional Requirements

The system should be:

- Scalable prioritizing consistency and partition tolerance.
- Designed to handle millions of concurrent connections with high availability and minimal latency.

# Infrastructure

- `Scalability`: using a NoSQL database, servers can be scaled horizontally across multiple regions more easily than SQL databases while avoiding JOIN operations.
- `Database`: from the CAP theorem, prioritizing [consistency and partition tolerance](https://github.com/camponogaraviera/full-stack-roadmap/blob/dev/database/04_CAP_theorem.md) implies that [MongoDB](https://github.com/camponogaraviera/full-stack-roadmap/blob/dev/database/06_technologies/MongoDB.md) or [DynamoDB]([https://aws.amazon.com/pm/dynamodb](https://github.com/camponogaraviera/full-stack-roadmap/blob/dev/database/06_technologies/DynamoDB.md)) can be used.
- `Availability`: to make the system available, one can replicate the database across multiple servers and use cold/warm/hot standbys. In AWS, [global tables](https://aws.amazon.com/dynamodb/global-tables/) and [DynamoDB Streams](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Streams.html) can be used to enable database replication across multiple regions.
- `API Gateway:` to provide a single entry point for clients to interact with various backend services, handling tasks such as geo-routing, RESTful API definitions, and access control. It also includes a load balancer to distribute incoming traffic (requests) across a fleet of servers.
- `Backend API`: The API for client-server communication can be implemented with [RESTful API](https://github.com/camponogaraviera/full-stack-roadmap/blob/dev/backend/restfull-api.md) (through [AWS API Gateway](https://aws.amazon.com/api-gateway/)+[Lambda](https://aws.amazon.com/lambda/)), [GraphQL](https://github.com/camponogaraviera/full-stack-roadmap/blob/dev/backend/grahql.md) (through [AWS Amplify](https://aws.amazon.com/amplify/) + [AppSync](https://aws.amazon.com/appsync/)), or [gRPC](https://github.com/camponogaraviera/full-stack-roadmap/blob/dev/backend/gRPC.md).
- `Deployment`: if more control over the infrastructure is required, `Kubernetes` might be a good choice. It is also possible to go with a serverless application using auto-scale provisioned capacity in AWS. Check out [serverless services](https://aws.amazon.com/serverless/) on AWS.

# Architecture

Microservices architecture, where each service is isolated and self-contained, might be a good starting point for horizontal scalability.

# Services

1. `Frontend Service:` to manage the UX/UI.
2. `Web Crawler Service:` to fetch web pages and store raw crawled data in a distributed file system (e.g., [Hadoop HDFS](https://hadoop.apache.org/docs/r1.2.1/hdfs_design.html) or [AWS S3](https://aws.amazon.com/s3/)).
3. `Parser Service:` to parse the raw HTML data fetched by the crawler to extract meaningful content such as text, links, and metadata.
4. `Storage Service`: [AWS S3](https://aws.amazon.com/s3/) can be used to store parsed data, and indexes.
5. `Indexer`: to map documents to keywords, positions, and other signals. This can be implemented with a NoSQL key-value database ([DynamoDB](https://github.com/camponogaraviera/full-stack-roadmap/blob/dev/database/06_technologies/DynamoDB.md) or [Bigtable](https://cloud.google.com/bigtable)).
6. `Query Service:` to handle user search queries, retrieving and returning relevant documents from the index.
7. `Ranking Service:` to rank documents retrieved by the query service based on their relevance to the user's search keyword. TF-IDF (Term Frequency-Inverse Document Frequency) is not a suitable choice if searching is to be made in a large corpus (the entire web).
8. `Inverted Index Service:` to map keywords to a list of documents containing those keywords. This can be implemented with a NoSQL key-value database ([DynamoDB](https://github.com/camponogaraviera/full-stack-roadmap/blob/dev/database/06_technologies/DynamoDB.md) or [Bigtable](https://cloud.google.com/bigtable)).
9. `Caching Service`: can be used to `reduce latency` by caching the most frequently queried search terms, web pages, and search indexes. Technologies: [AWS DAX](https://aws.amazon.com/pt/dynamodb/dax/) and [Amazon Elasticache](https://aws.amazon.com/pm/elasticache).

# Estimations

## Traffic

Suppose the system has 500k DAUs, and each user makes 10 searches per day on average.

- Search requests per day:

$$
500k \space (users) \times 10 \space (searches) = 5M \space searches/day
$$

- Number of Requests Per Second (RPS): 

$$
\frac{5M}{(24 \times 3600 \space seconds)} \sim 58 \space RPS
$$

## Storage

Suppose 1M websites of 1MB are crawled per day, on average. 

- Daily storage usage = 1M websites  * 0.001GB/website = 1,000 GB/day or 1 TB/day.
- Monthly storage usage = 1 TB/day * 30 days = 30 TB/month.

## Bandwidth

The Bandwidth (data transfer) considering an ingress of 1 TB of data stored per day:

$$
\frac{1 \space TB}{(24 \times 3600 \space seconds)} \sim 12 \space MB/second
$$
