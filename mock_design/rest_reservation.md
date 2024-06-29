<div align='center'>
  <h1>Design a Restaurant Reservation System</h1>
</div>

# Functional Requirements

The user should be able to:

- Log in with email and password.
- Search for a restaurant.
- Make a reservation.
- Choose the reservation date and time.
- Specify the reservation length. 
- Specify the party size.
- Specify dietary restrictions.
- Cancel a reservation.
- Choose a payment method.
- Receive an email confirmation.
- Rate a restaurant and submit reviews/feedback.
- Save favorite restaurants.

The restaurant owner should be able to:

- Register a restaurant.
- Upload images and videos of the restaurant.
- Receive booking confirmations, with details of schedule, party size, and reservation length.
- Receive online payments.
- Get analytics and feedback from customers.

# Non-Functional Requirements

The system should be:

- Scalable prioritizing Consistency and Partition-Tolerance.
- Reliable (fault-tolerant without losing uploads).
- Designed to handle millions of concurrent connections with fast loading times and minimal latency.
 
# Infrastructure

- `Scalability`: using a NoSQL database, servers can be scaled horizontally across multiple regions more easily than SQL databases while avoiding JOIN operations. [Sharding](https://github.com/camponogaraviera/full-stack-roadmap/blob/dev/system_design_and_infrastructure/03_horizontal_scaling.md) can be achieved using a partition key such as `restaurant_id`.
- `Database`: from the CAP theorem, prioritizing [consistency and partition tolerance](https://github.com/camponogaraviera/full-stack-roadmap/blob/dev/database/04_CAP_theorem.md) implies that [MongoDB](https://github.com/camponogaraviera/full-stack-roadmap/blob/dev/system_design_and_infrastructure/database/06_technologies/MongoDB.md) or [DynamoDB](https://github.com/camponogaraviera/full-stack-roadmap/blob/dev/system_design_and_infrastructure/database/06_technologies/DynamoDB.md) can be used.
- `Availability`: to make the system available, one can replicate the database across multiple servers and use cold/warm/hot standbys. In AWS, [global tables](https://aws.amazon.com/dynamodb/global-tables/) and [DynamoDB Streams](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Streams.html) can be used to enable database replication across multiple regions.
- `API Gateway:` to provide a single entry point for clients to interact with various backend services, handling tasks such as geo-routing, RESTful API definitions, and access control. It also includes a load balancer to distribute incoming traffic (requests) across a fleet of servers.
- `Backend API`: The API for client-server communication can be implemented with [RESTful API](https://github.com/camponogaraviera/full-stack-roadmap/blob/dev/system_design_and_infrastructure/backend/restfull_api.md) (through [AWS API Gateway](https://aws.amazon.com/api-gateway/)+[Lambda](https://aws.amazon.com/lambda/)), [GraphQL](https://github.com/camponogaraviera/full-stack-roadmap/blob/dev/backend/grahql.md) (through [AWS Amplify](https://aws.amazon.com/amplify/) + [AppSync](https://aws.amazon.com/appsync/)), or [gRPC](https://github.com/camponogaraviera/full-stack-roadmap/blob/dev/backend/gRPC.md).
- `Deployment`: if more control over the infrastructure is required, `Kubernetes` might be a good choice. It is also possible to go with a serverless application using auto-scale provisioned capacity in AWS. Check out [serverless services](https://aws.amazon.com/serverless/) on AWS.

# Architecture

Microservices architecture, where each service is isolated and self-contained, might be a good starting point for horizontal scalability.

# Services

1. `Frontend Service:` the UX/UI can be implemented with JavaScript and React Native.
2. `User Service`: login, authentication, and authorization can be implemented with [AWS Cognito](https://aws.amazon.com/pm/cognito/) and [AWS IAM](https://aws.amazon.com/iam/), respectively.
3. `Storage Service`: [AWS S3](https://aws.amazon.com/s3/) can be used to store raw image files with auto replication.
4. `Search Engine Service:` the search engine can be implemented with: [Elasticsearch](https://www.elastic.co/elasticsearch), [AWS OpenSearch](https://aws.amazon.com/what-is/opensearch/), [AWS CloudSearch](https://aws.amazon.com/cloudsearch/), or `Unicorn` (Facebook).
5. `URL Sharing Service:` users can share their favorite restaurants using a `URL shortener service`.
6. `Payments Service:` payment transactions can be processed with `Strype` or `PayPal`.
7. `Email Confirmation Service:` after a reservation is successfully created, an email confirmation template with relevant reservation details can be triggered via an [AWS Lambda function](https://aws.amazon.com/lambda/). Customers and restaurant owners can then receive the email from an SMTP server (e.g. using `Nodemailer`) or through an email service provider's API (e.g., [Amazon SES API](https://docs.aws.amazon.com/ses/latest/dg/send-email-api.html) or `SendGrid`.
8. `Metrics and Analytics Service:` DynamoDB does not support analytical operations out of the box. One solution is to combine services such as [DynamoDB Streams](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Streams.html) and [Amazon Kinesis Data Analytics](https://docs.aws.amazon.com/kinesisanalytics/latest/dev/what-is.html) to process and aggregate data in real-time. A custom Lambda function can be set up to be triggered whenever data changes in a specific DynamoDB table to be analyzed. [Amazon QuickSight](https://docs.aws.amazon.com/managedservices/latest/userguide/quicksight.html) can be used to create dashboards for analytics, data visualization, and reporting.
9. `CDN Service`: can be used to store static content (restaurant images and videos) on servers geographically closer to end users for `fast loading times`. The CDN takes the static content from the AWS S3 Storage. Technologies: [AWS CloudFront](https://aws.amazon.com/cloudfront/)
10. `Caching Service`: can be used to `reduce latency` by caching the most frequent database queries to the Table attributes that are not likely to change too often. Technologies: [AWS DAX](https://aws.amazon.com/dynamodb/dax/) (purpose-built for DynamoDB), [Amazon Elasticache](https://aws.amazon.com/pm/elasticache/) (general-purpose). This caching solution stays in the AWS cloud.
   
# Estimations

## Traffic

Suppose the system has 500k DAUs, and each user makes 10 requests for a particular location per day, on average.

- Search requests per day:

$$
500k \space (users) \times 10 \space (searches) = 5M \space searches/day
$$

- Number of Requests Per Second (RPS): 

$$
\frac{5M}{(24 \times 3600 \space seconds)} \sim 58 \space RPS
$$

## Storage

Suppose that 10% of DAU are restaurant owners sharing 5 photos of 1MB per day (0.005GB/user), on average. 

- Daily storage usage for 50k DAU = 50k users * 0.005GB/user = 250 GB/day.
- Monthly storage usage = 250 GB/day * 30 days = 7,5 TB/month.

## Bandwidth

The Bandwidth (data transfer) considering an ingress of 250 GB of data stored per day:

$$
\frac{250 \space GB}{(24 \times 3600 \space seconds)} \sim 2.9 \space MB/second
$$

# Identifying Potential Bottlenecks

## Double Booking

To avoid double booking, an idempotency key (a.k.a unique key) can be used. In the case of a restaurant booking system, this can be a reservation key (`reservation_id`) generated by the backend during checkout. The client then sends this key to a POST API endpoint that will update the reservation status in the database.

## Database Contention

Consider the following two examples:

1) Two customers get the same flight seat, hotel room, or restaurant reservation slot. 
2) A video goes viral and millions of concurrent transactions are sent to the "videos" table to update the `videos.views` counter.

These are examples of parallel request problems, often referred to as `database contention`, where multiple processes attempt to update the same row of "locked" data simultaneously. Recall from ACID transactions that isolation means that transactions operate independently without interference from concurrent transactions. One solution is `locking with serialization`. [Serializable isolation](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/transaction-apis.html#transaction-isolation) in database transactions enables concurrent operations (parallel transactions) while ensuring that the result of those transactions is identical to if they were executed sequentially, without concurrency. `Locks` are often used to enforce isolation by preventing multiple transactions from accessing or modifying the same data simultaneously. 

# Entity-Relationship Diagram (ERD)

...

# Entity Chart

- In a relational database, each entity is represented by a table. 
- In a NoSQL database, entities are the nouns of the application.

A minimal version of the system has the following entity chart:

|        Entity       |   Primary Key: PK   |   Sort Key: SK    |              
|---------------------|---------------------|-------------------|
| User                |   USER#username     | USER#username     |
| Restaurants         |   REST#RestaurantID | REST#RestaurantID |               
| Bookings            |   USER#username     | BOOK#BookingID    |
| Payments            |   USER#username     | PAY#PaymentID     |
| Reviews             |   REST#RestaurantID | REV#ReviewID      |
| Location            |   LOC#LocationID    | REST#RestaurantID |

#  SQL Database Design (normalized data)

1. Users Table:
	- Primary Key: userID (uuid)
 	- name (varchar)
	- email (varchar)
 	- phoneNumber (varchar) 
  	- favouriteRestaurants (JSON)
	- role (varchar) - customer or restaurant owner
 	- createdAt (timestamp) 

2. Restaurants Table:
 
	- Primary Key: restaurantID (uuid) 
 	- userID (uuid) - Foreign Key for owner's id referencing Users table
	- name (varchar)
	- description (varchar)
 	- address (varchar or point)
  	- ContactInfo (JSON)
	- CuisineType (varchar)
	- OpeningHours (varchar)
 	- availableTables (JSON) - available tables and seats per table
	- availableCreditCards (JSON)
	- imageURL (varchar or JSON) - JSON array for storing image URLs
	- videoURL (varchar or JSON) - JSON array for storing video URLs

3. Bookings Table:

	- Primary Key: bookingID (uuid) 
	- userID (uuid) - Foreign Key referencing Users table
	- restaurantID (uuid) - Foreign Key referencing Restaurants table
	- reservationDate (timestamp) 
	- partySize (int)
   	- reservationLength (int)
 	- dietaryRestrictions (varchar)
  	- reservationStatus (enum) - Pending/Confirmed/Cancelled

4. Payments Table:

	- Primary Key: paymentID (uuid) 
	- bookingID (uuid) - Foreign Key referencing Bookings table
 	- transactionID (uuid)
  	- paymentMethod (enum) - Visa, Mastercard, PayPal, etc.
	- amount (float)
 	- paymentStatus (enum) - Completed/Denied
	- paymentDate (timestamp)
 
5. Reviews Table:

	- Primary Key: reviewID (uuid)
	- userID (uuid) - Foreign Key referencing the Users table
	- restaurantID (uuid) - Foreign Key referencing Restaurants table 
	- rating (int)
	- feedback (varchar)
	- reviewDate (timestamp)

# DynamoDB [Access (Query) Patterns](https://docs.aws.amazon.com/prescriptive-guidance/latest/dynamodb-data-modeling/step3.html)

Access patterns are required to be known before modeling DynamoDB Tables. Recall that when there is more than one entity (item) and a `fetch many` access pattern, a composite primary key should be used to satisfy the required access pattern. 

1. Register a User unique on both username and email address (**not required when using Amazon Cognito**):
	- Partition Key (PK): `USER#username`
 	- Sort key (SK): `USER#username`
 	- Attributes: `username`, `email`, `name`, `role`, etc.
 	- Post: use `TransactWriteItems` to create an item with `PK` and `SK` both as `USER#username`.
 	
2. Register a Restaurant (unique on ID):
	- Partition Key (PK): `REST#RestaurantID`
 	- Sort key (SK): `REST#RestaurantID`
	- Attributes: `restaurantName`, `address`, `cuisineType`, etc.
 	- Post: use `PutItem` with `PK` and `SK` both as `REST#RestaurantID`.
	
3. Book a Restaurant: 
	- Partition Key (PK): `USER#username`
 	- Sort key (SK): `#BOOK#BookingID` (KSUID)
 	- Attributes: `restaurantID`, `reservationDate`, `reservationLength`, `partySize`, `dietaryRestrictions`, `reservationStatus`.
 	- Post: use `TransactWriteItems` with `PK` as `USER#username` and `SK` as `BOOK#BookingID`.
 	
4. Update a Booking:
	- Partition Key (PK): `USER#username`
 	- Sort key (SK): `BOOK#BookingID`
 	- Put: use `UpdateItem` with `PK` as `USER#username` and `SK` as `BOOK#BookingID`.
 	
5. Fetch the most recent Bookings for a particular User: 
	- Partition Key (PK): `USER#username`
 	- Sort key (SK): `BOOK#BookingID`
   	- Attributes: `restaurantID`, `reservationDate`, `partySize`, `dietaryRestrictions`, `reservationStatus`.
 	- Get: use `Query` with `ScanIndexForward=False`, `PK` as `USER#username` and a sort key condition starting with `BOOK#`.
 	
6. Fetch all Restaurants from a particular User: 
	- Partition Key (PK): `USER#username`
 	- Sort key (SK): `REST#RestaurantID`
  	- Attributes: `restaurantName`, `address`, `cuisineType`, `ownerEmail`, etc.
 	- Get: use `Query` with `PK` as `USER#username` and a sort key condition starting with `REST#`.
 	
7. Fetch all Restaurants from a particular Location: 
	- Partition Key (PK): `LOC#LocationID`
 	- Sort key (SK): `REST#RestaurantID`
  	- Attributes: `restaurantID`, `restaurantName`, `address`, `cuisineType`, `ownerEmail`, etc.
 	- Get: use `Query` with `PK` as `LOC#LocationID` and a sort key condition starting with `REST#`.

8. Fetch all Payments from a particular User: 
	- Partition Key (PK): `USER#username`
 	- Sort key (SK): `PAY#PaymentID`
	- Attributes: `date`, `amount`, `status`, `bookingID`, etc.
 	- Get: use `Query` with `PK` as `USER#username` and a sort key condition starting with `PAY#`.
 	
9. Fetch all Reviews from a particular Restaurant: 
	- Partition Key (PK): `REST#RestaurantID`
 	- Sort key (SK): `REV#ReviewID`
	- Attributes: `Username`, `Rating`, `Feedback`, etc.
 	- Get: use `Query` with `PK` as `REST#RestaurantID` and a sort key condition starting with `REV#`.
 	 	
## DynamoDB Table (denormalized data)

- As discussed in the DynamoDB section, contrary to relational databases, DynamoDB does not support join operations by design. While not recommended, it is possible to simulate/fake a join by performing multiple key-value lookups. For example, one might first fetch a record and then make additional requests to retrieve related records. This pattern should be avoided as much as possible since `network I/O introduces latency and multiple queries increase computational (CPU) cost`. Ideally, one should design a denormalized single table for each microservice/application. With Moore's law closing to an end, CPU operations will end up being more expensive than memory storage on hard drives. Therefore, data redundancy introduced in a single-table design is justifiable as the system scales. The design philosophy of DynamoDB emphasizes a `single-table model through data denormalization to avoid the need for multiple queries`. By using a composite primary key and a single-table design, one can effectively "join" related data without performing multiple queries.

- Recall that access patterns such as `fetch user and bookings from user` (one-to-many relationship) require a `pre-join`, i.e., the Booking items should belong to the same partition key (same item collection) as the parent item (e.g., USER#username). This allows for fetching Users and Bookings in a single request. The same applies to fetching restaurants from locations, etc.

- In the following table, PK and SK are used as generic names to represent the partition key and sort key, respectively. This is called `primary key overloading`. Also, a prefix (e.g., USER#, REST#, REV#, PAY#) is added to each value of a partition key and sort key to avoid overlapping between items with the same name (e.g., a user and a restaurant with the same name), since a primary key must be unique across items. And the prefix `#` in #BOOK# is used to ensure that bookings are fetched in descending order. 

<table>
  <tr> 
    <td colspan="2"> Primary Key </td>
    <td colspan="6">Attributes</td>
  </tr>
    <th>Partition Key: PK</th>
    <th>Sort Key: SK</th>
    <th>Attribute 1</th>
    <th>Attribute 2</th>
    <th>Attribute 3</th>
    <th>Attribute 4</th>
    <th>Attribute 5</th>
    <th>Attribute 6</th>
  </tr>
  <tr>  
  </tr>	
  <tr>
    <td rowspan="4">Item Collection<br>USER#sherlock</td>
    <td colspan="1">  </td>
    <td>restaurantName</td>
    <td>address</td>
    <td>cuisineType</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>REST#221</td>
    <td>The Mazarin Stone</td>
    <td>{Street: 221B Baker}</td>
    <td>"Vegan" </td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td colspan="1">  </td>
    <td> Username </td>
    <td> Email </td>
    <td> Name </td>
    <td> Role </td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>USER#sherlock</td>
    <td>sherlock</td>
    <td>sherlock@example.com</td>
    <td>Sherlock Holmes</td>
    <td>Restaurant Owner</td>
    <td></td>
    <td></td>
  </tr>

  <tr>
    <td rowspan="6">Item Collection<br>USER#jamesbond</td>
    <td colspan="1">  </td>
    <td>restaurantName</td>
    <td>address</td>
    <td>cuisineType</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>REST#007</td>
    <td>Goldfinger</td>
    <td>{Street: 007B Square}</td>
    <td>"English" </td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td colspan="1">  </td>
    <td> restaurantID </td>
    <td> reservationDate </td>
    <td> reservationLength </td>
    <td> partySize </td>
    <td> dietaryRestrictions </td>
    <td> reservationStatus </td>
  </tr>
  <tr>
    <td>#BOOK#001</td>
    <td>001</td>
    <td>2024-06-01T10:00:00Z</td>
    <td>"1 hour"</td>
    <td>"Table for 2"</td>
    <td> "Peanut Allergy" </td>
    <td>"Pending"</td>
  </tr>
  <tr>
    <td colspan="1">  </td>
    <td> Username </td>
    <td> Email </td>
    <td> Name </td>
    <td> Role </td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>USER#jamesbond</td>
    <td>jamesbond</td>
    <td>jamesbond@example.com</td>
    <td>James Bond</td>
    <td>Restaurant Owner</td>
    <td></td>
    <td></td>
  </tr>
  
  <tr>
    <td rowspan="4">Item Collection<br>LOC#LDN</td>
    <td colspan="1">  </td>
    <td>restaurantID</td>
    <td>restaurantName</td>
    <td>address</td>
    <td>cuisineType</td>
    <td>ownerEmail</td>
    <td></td>
  </tr>
  <tr>
    <td>REST#221</td>
    <td>221</td>
    <td>The Mazarin Stone</td>
    <td>{Street: 221B Baker}</td>
    <td>"Vegan" </td>
    <td>sherlock@example.com</td>
    <td></td>
  </tr>
  <tr>
    <td colspan="1">  </td>
    <td>restaurantID</td>
    <td>restaurantName</td>
    <td>address</td>
    <td>cuisineType</td>
    <td>ownerEmail</td>
    <td></td>
  </tr>
  <tr>
    <td>REST#007</td>
    <td>007</td>
    <td>Goldfinger</td>
    <td>{Street: 007B Square}</td>
    <td>"English" </td>
    <td>jamesbond@example.com</td>
    <td></td>
  </tr>
 
  <tr>
    <td rowspan="2">USER#jamesbond</td>
    <td colspan="1">  </td>
    <td>date</td>
    <td>amount</td>
    <td>status</td>
    <td>bookingID</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>PAY#001</td>
    <td>2024-06-01T10:00:00Z</td>
    <td>50</td>
    <td>Completed</td>
    <td>001</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td rowspan="2">REST#221</td>
    <td colspan="1">  </td>
    <td> Username </td>
    <td> Rating </td>
    <td> Feedback </td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>REV#001</td>
    <td>jamesbond</td>
    <td>5</td>
    <td>"Superb!"</td>
    <td></td>
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
