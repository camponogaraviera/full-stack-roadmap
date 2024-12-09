<div align='center'>
    <h1> Patterns for Enabling Data Persistence </h1>
</div>

# Table of Contents

- Saga Pattern
  - Serverless Implementation
- Database-Per-Service Pattern
- Shared-Database-Per-Service Pattern
- API Composition Pattern
- CQRS Pattern
- Event Sourcing Pattern

# Saga Pattern

The [saga pattern](https://docs.aws.amazon.com/prescriptive-guidance/latest/modernization-data-persistence/saga-pattern.html) is a failure management pattern used to maintain data consistency in a distributed system/application where business transactions span multiple microservices. It is `an alternative to distributed transactions` avoiding the complexity and overhead associated with protocols such as [Two-Phase Commit (2PC)]() and [Three Phrase Commit (3PC)](). 

In a [microservices architecture](https://aws.amazon.com/microservices/#:~:text=Microservices), where each service is decoupled (isolated) and self-contained with its own data persistence layer (a `database-per-service pattern`), a single/local ACID transaction cannot span multiple services because each service manages its own database. Attempting to use a local ACID transaction across multiple services can result in partial distributed transactions, where only some operations succeed while others fail, potentially leading to data inconsistencies. The saga pattern is used to prevent these issues to happen.

The core idea behind the saga pattern is to break down a distributed transaction into a series of smaller, `coordinated local transactions` in a way that each step can either complete successfully or be compensated (reverted to a previous step) if a transaction fails at some point in the process, maintaining data consistency. Each step then becomes ACID-compliant. 

Transactions can be coordinated in two ways:

- Choreography-based Saga: a local transaction triggers another local transaction. Microservices communicate with each other through events. Use case: a restaurant "Reservation Confirmed" event triggers a payment, then a "Payment Processed" event triggers a notification.

- Orchestration-based Saga: A central orchestrator (e.g., AWS Step Functions) coordinates the entire workflow by calling each microservice and ensuring each step is completed before moving to the next.

- Notes:
  - A local transaction is a sequence of database operations performed as a single unit within a single database. This ensures that all operations are successfully completed (committed) or none of them are (rolled back) if an error occurs, maintaining ACID compliance.
  - A distributed transaction is a sequence of coordinated database operations that span (are performed on) multiple databases, often located across different servers. These transactions can be coordinated with protocols such as Two-Phase Commit (2PC) and Three Phrase Commit (3PC).

## Examples 

Use a SAGA pattern:

1. If you have multiple services that must maintain consistency:
   - Payment service (e.g., Stripe).
   - Calendar service (e.g., Google Calendar integration).
   - Notification service.
   - Video conferencing service (e.g., Zoom meeting creation).
   
2. If you need complex rollback operations:
   - Refund payments.
   - Release calendar slots.
   - Notify multiple parties.
   - Cancel created meetings.

## Serverless Implementation

The saga workflow can be implemented using serverless components on AWS. `AWS API Gateway`, `AWS Step Functions`, `AWS Lambda`, and `Amazon DynamoDB` can be orchestrated to implement the saga pattern.

Use case: [Implement the serverless saga pattern by using AWS Step Functions](https://docs.aws.amazon.com/prescriptive-guidance/latest/patterns/implement-the-serverless-saga-pattern-by-using-aws-step-functions.html).

# Database-Per-Service Pattern

Using the database-per-service pattern in a microservices architecture ensures that each service can manage its own database independently while still participating in a coordinated workflow. This allows for better isolation and scalability. In DynamoDB, tables are independent without relying on JOIN operations or foreign keys, which fits the database-per-service model.

In [this example](https://docs.aws.amazon.com/prescriptive-guidance/latest/modernization-data-persistence/database-per-service.html), a system with three microservices (sales, costumer, compliance) uses one dedicated database for each service to align with the database-per-service pattern. There are no restrictions on the choice of the database, different microservices can use different databases (SQL or NoSQL) depending on the service's requirement.

# Shared-Database-Per-Service Pattern

In this pattern, multiple services share a common database instance, even if they have separate tables within that database. In an RDS, multiple tables within a single database instance share infrastructure and relational features such as JOIN operations and foreign keys, fitting the shared-database-per-service model.

In [this example](https://docs.aws.amazon.com/prescriptive-guidance/latest/modernization-data-persistence/shared-database.html), having three tables (sales, customer, and compliance) within a single RDS database aligns with the shared-database-per-service pattern. While each service may have its own table, they all share the same underlying database instance.

# API Composition Pattern

# CQRS Pattern

# Event Sourcing Pattern
