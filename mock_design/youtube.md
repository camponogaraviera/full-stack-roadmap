<div align='center'>
  <h1>Design a YouTube-like On-Demand Video Streaming Service</h1>
</div>

# Functional Requirements

The user should be able to:

- Sign up and Sign in with email and password.
- Upload a video.
- Search for a video.
- Stream and watch a video-on-demand or live.
- Submit reviews/feedback.

# Non-Functional Requirements

The system should be:

- Scalable, prioritizing Availability and Partition-Tolerance.
- Reliable (fault-tolerant without losing uploads).
- Designed to handle millions of concurrent connections with fast loading times and minimal latency.

# Infrastructure

- `Scalability`: using a NoSQL database, servers can be scaled horizontally across multiple regions more easily than SQL databases while avoiding JOIN operations.
- `Database`: video metadata and user interactions (ownership, likes, comments) can be stored in a NoSQL database. From the CAP theorem, prioritizing [availability and partition tolerance](https://github.com/camponogaraviera/full-stack-roadmap/blob/dev/system_design_and_infrastructure/database/04_CAP_theorem.md)  implies that `Cassandra` can be used.
- `Availability`: to make the system available, one can replicate the database and use cold/warm/hot standbys.
- `API Gateway:` to provide a single entry point for clients to interact with various backend services, handling tasks such as geo-routing, RESTful API definitions, and access control. It also includes a load balancer to distribute incoming traffic (requests) across a fleet of servers.
- `Backend API`: The API for client-server communication can be implemented with [RESTful API](https://github.com/camponogaraviera/full-stack-roadmap/blob/dev/system_design_and_infrastructure/backend/restfull_api.md) (through [AWS API Gateway](https://aws.amazon.com/api-gateway/)+[Lambda](https://aws.amazon.com/lambda/)), [GraphQL](https://github.com/camponogaraviera/full-stack-roadmap/blob/dev/backend/grahql.md) (through [AWS Amplify](https://aws.amazon.com/amplify/) + [AppSync](https://aws.amazon.com/appsync/)), or [gRPC](https://github.com/camponogaraviera/full-stack-roadmap/blob/dev/backend/gRPC.md).
- `Deployment`: if more control over the infrastructure is required, `Kubernetes` might be a good choice. It is also possible to go with a serverless application using auto-scale provisioned capacity in AWS. Check out [serverless services](https://aws.amazon.com/serverless/) on AWS.

# Architecture

Microservices architecture, where each service is isolated and self-contained, might be a good starting point for horizontal scalability.

# Services

1. `Frontend Service:` the UX/UI can be implemented with JavaScript and React Native.
2. `User Service:` login, authentication, and authorization can be implemented with [AWS Cognito](https://aws.amazon.com/pm/cognito/) and [AWS IAM](https://aws.amazon.com/iam), respectively.
3. `Storage Service`: [AWS S3](https://aws.amazon.com/s3/) can be used to store both raw and encoded (post-processed) video files with auto replication.
4. `Search Engine Service:` the search engine can be implemented with: `Elasticsearch`, [AWS OpenSearch](https://aws.amazon.com/what-is/opensearch/), [AWS CloudSearch](https://aws.amazon.com/cloudsearch/), or `Unicorn` (Facebook).
5. `Media Service:` for video upload and pre-processing (encoding, transcoding), a message queue (AWS SQS) can be used for encoding/transcoding requests and to send videos to a domain-specific service (possibly a fleet of servers). Post-processed videos are then stored in the same object storage as the raw videos. Available AWS services are [Amazon Elastic Transcoder](https://aws.amazon.com/elastictranscoder/) and [AWS Elemental MediaConvert](https://aws.amazon.com/mediaconvert/).
6. `Streaming Service:` on-demand video streaming with playback feature using offset can be implemented with [AWS Elemental MediaPackage](https://aws.amazon.com/pt/mediapackage/) (has support for DVR features). For live streaming, [AWS Elemental MediaStore](https://aws.amazon.com/mediastore/) and [AWS Kinesis Video Streams](https://aws.amazon.com/kinesis/video-streams/?amazon-kinesis-video-streams-resources-blog.sort-by=item.additionalFields.createdDate&amazon-kinesis-video-streams-resources-blog.sort-order=desc) are available options.
7. `URL Sharing Service:` users can share their favorite videos using a `URL shortener service`.
8. `Caching Service`: can be used to `reduce latency` by caching the most frequent database queries to the Table attributes that are not likely to change too often. Technologies: [AWS DAX](https://aws.amazon.com/dynamodb/dax/) (purpose-built for DynamoDB) and [Amazon Elasticache](https://aws.amazon.com/pm/elasticache).
9. `CDN Service`: can be used to store static content (viral videos) on servers geographically closer to end users for `fast loading times`, thus avoiding the `celebrity problem`. The CDN takes the viral videos from the AWS S3 Storage. Technologies: [AWS CloudFront](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html).

# Estimations

## Traffic

Suppose the system has 500k DAUs, and each user searches/watches 10 videos per day on average.

- Search requests per day:

$$
500k \space (users) \times 10 \space (searches) = 5M \space searches/day
$$

- Number of Requests Per Second (RPS): 

$$
\frac{5M}{(24 \times 3600 \space seconds)} \sim 58 \space RPS
$$

## Storage

Suppose each user uploads 5 videos of 100MB per day (0.5GB/user), on average. 

- Daily storage usage for 500k DAU = 500k users * 0.5GB/user = 250,000 GB/day or 250 TB/day.
- Monthly storage usage = 250 TB/day * 30 days = 7,500 TB/month.

## Bandwidth

The Bandwidth (data transfer) considering an ingress of 250 TB of data stored per day:

$$
\frac{250 \space TB}{(24 \times 3600 \space seconds)} \sim 2,894 \space MB/second
$$

# Entity-Relationship Diagram (ERD)

...

# Entity Chart

- In a relational database, each entity is represented by a table. 
- In a NoSQL database, entities are the nouns of the application.

A minimal version of the system has the following entity chart:

|       Entity        |  Primary Key: PK  | Sort Key: SK  |              
|---------------------|-------------------|---------------|
| User                |   USER#username   | USER#username |
| Videos              |   VIDEO#VideoID   | VIDEO#VideoID |
| Reviews             |   VIDEO#VideoID   | REV#ReviewID  |

#  SQL Database Design (normalized data)

1. Users Table:
	- Primary Key: userID (uuid)
 	- name (varchar)
	- email (varchar)
	- role (varchar) - Member or Admin
 	- createdAt (timestamp) 
 	
2. Videos Table:
	- Primary Key: videoID (uuid) 
 	- userID (uuid) - Foreign Key referencing Users table
	- videoTitle (varchar)
	- description (varchar)
	- thumbURL (varchar or JSON) - JSON array for storing thumbnail URL
	- videoURL (varchar or JSON) - JSON array for storing video URL
 	- createdAt (timestamp) 
 	
3. Reviews Table:
	- Primary Key: reviewID (uuid)
	- userID (uuid) - Foreign Key referencing the Users table
	- videoID (uuid) - Foreign Key referencing Videos table 
	- rating (int)
	- feedback (varchar)
	- reviewDate (timestamp)
 	
# DynamoDB [Access (Query) Patterns](https://docs.aws.amazon.com/prescriptive-guidance/latest/dynamodb-data-modeling/step3.html)

Access patterns are required to be known before modeling DynamoDB Tables. Recall that when there is more than one entity and a `fetch many` access pattern, a composite primary key should be used to satisfy the required access pattern. 

1. Register a User/Sign up unique on both username and email address (**not required when using Amazon Cognito**):
	- Partition Key (PK): `USER#username`
 	- Sort key (SK): `USER#username`
 	- Attributes: `Username`, `Email`, `Name`, `Role`, `DateJoined`, etc.
 	- Post: use `PutItem` with `PK` and `SK` both as `USER#username`.

2. Sign in with email and password (**not required when using Amazon Cognito**):
	- GSI1:
		- Partition Key (PK): `email`
		- Sort Key (SK): `USER#username`
		- Query: use `Query` on GSI1 with `PK` as `email`. 
 	
3. Upload a Video: 
	- Partition Key (PK): `VIDEO#VideoID`
 	- Sort key (SK): `VIDEO#VideoID`
 	- Attributes: `VideoID`, `VideoTitle`, `Description`, `Timestamp`, `URL`, etc.
 	- Post: use `PutItem` with `PK` and `SK` both as `VIDEO#VideoID`.

4. Search for a Video: 
	- GSI2:
		- Partition Key (PK): `Description`
		- Sort Key (SK): `VIDEO#VideoID`
		- Query: use `Query` on GSI2 with `PK` as the partial or full video `Description`.
 	
5. Fetch all Videos from a particular User: 
	- Partition Key (PK): `USER#username`
 	- Sort key (SK): `VIDEO#VideoID`
 	- Attributes: `VideoID`, `VideoTitle`, `Description`, `Timestamp`, `URL`, etc.
 	- Get: use `Query` with `PK` as `USER#username` and a sort key condition starting with `VIDEO#`.
 	
6. Fetch all Reviews from a particular Video: 
	- Partition Key (PK): `VIDEO#VideoID`
 	- Sort key (SK): `REV#ReviewID`
   	- Attributes: `Username`, `Rating`, `Feedback`, etc.
 	- Get: use `Query` with `PK` as `VIDEO#VideoID` and a sort key condition starting with `REV#`.
 	
## DynamoDB Table (denormalized data)

- As discussed in the DynamoDB section, contrary to relational databases, DynamoDB does not support join operations by design. While not recommended, it is possible to simulate/fake a join by performing multiple key-value lookups. For example, one might first fetch a record and then make additional requests to retrieve related records. This pattern should be avoided as much as possible since `network I/O introduces latency and multiple queries increase computational (CPU) cost`. Ideally, one should design a denormalized single table for each microservice/application. With Moore's law closing to an end, CPU operations will end up being more expensive than memory storage on hard drives. Therefore, data redundancy introduced in a single-table design is justifiable as the system scales. The design philosophy of DynamoDB emphasizes a `single-table model through data denormalization to avoid the need for multiple queries`. By using a composite primary key and a single-table design, one can effectively "join" related data without performing multiple queries.

- Recall that access patterns such as `fetch a user and videos from user` requires a `pre-join`, i.e., the Video items should belong to the same partition key (same item collection) as the parent item (e.g., USER#username). This allows for fetching Users and Videos in a single request.

- In the following table, PK and SK are used as generic names to represent the partition key and sort key, respectively. This is called `primary key overloading`. Also, a prefix (e.g., USER#, VIDEO#, REV#) is added to each value of a partition key and sort key to avoid overlapping between items with the same name (e.g., a user and a video with the same name), since a primary key must be unique across items. And the prefix `#` in #VIDEO# is used to ensure that videos are fetched in descending order. 

<table>
  <tr> 
    <td colspan="2"> Primary Key </td>
    <td colspan="5">Attributes</td>
  </tr>
  <tr>
    <th>Partition Key: PK</th>
    <th>Sort Key: SK</th>
    <th>Attribute 1</th>
    <th>Attribute 2</th>
    <th>Attribute 3</th>
    <th>Attribute 4</th>
    <td>Attribute 5</td>
  </tr>
  <tr> 
  </tr> 
  <tr>
    <td rowspan="2">USER#jamesbond</td>
    <td colspan="1">  </td>
    <td> Username </td>
    <td> Email </td>
    <td> Name </td>
    <td> Role </td>
    <td> DateJoined </td>
  </tr>
  <tr>
    <td>USER#jamesbond</td>
    <td>"jamesbond"</td>
    <td>"jamesbond@example.com"</td>
    <td>"James Bond"</td>
    <td>"admin"</td>
    <td>2024-06-01T00:00:00Z</td>
  </tr>
  <tr>
    <td rowspan="4">Item Collection<br>USER#sherlock</td>
    <td colspan="1">  </td>
    <td>VideoID</td>
    <td>VideoTitle</td>
    <td>Description</td>
    <td>Timestamp</td>
    <td>URL</td>
  </tr>
  <tr>
    <td>#VIDEO#001</td>
    <td>001</td>
    <td>"Sherlock Holmes"</td>
    <td>"Granada Television"</td>
    <td>2024-06-01T10:00:00Z</td>
    <td>{url: '...'}</td>
  </tr>
  <tr>
    <td colspan="1">  </td>
    <td> Username </td>
    <td> Email </td>
    <td> Name </td>
    <td> Role </td>
    <td> DateJoined </td>
  </tr>
  <tr>
    <td>USER#sherlock</td>
    <td>"sherlock"</td>
    <td>"sherlock@example.com"</td>
    <td>"Sherlock Holmes"</td>
    <td>"user"</td>
    <td>2024-06-01T00:00:00Z</td>
  </tr>
  <tr>
    <td rowspan="2">VIDEO#001</td>
    <td colspan="1">  </td>
    <td> Username </td>
    <td> Rating </td>
    <td> Feedback </td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>REV#001</td>
    <td>"jamesbond"</td>
    <td>5</td>
    <td>"Superb!"</td>
    <td></td>
    <td></td>
  </tr>
</table>

# AWS Workflow

- Workflow: 
  - Web & Mob Applications, IoT devices => 
    - => API Gateway (load balancer, geo-routing, RESTful API definitions, and access control)
      - => [Amazon Cognito](https://aws.amazon.com/cognito/) (CIAM such as sign up, sign in, IAM roles, etc.)
      - => [AWS IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/intro-structure.html) (user access/authorization to different AWS services)
      - => AWS Lambda (functions)
        - => DynamoDB (database) => DynamoDB Stream (logging DynamoDB changes)
        - => AWS Lambda => Amazon SNS (mobile push notifications)
        - ...

## AWS Data Storage Cost

Try the [AWS S3 Calculator](https://calculator.aws/#/addService) to get a quotation for each AWS service used.
