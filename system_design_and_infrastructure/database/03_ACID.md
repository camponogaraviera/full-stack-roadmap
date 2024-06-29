<div align='center'>
  <h1>ACID Properties</h1>
</div>

# Table of Contents

- About
- Atomicity
- Consistency
- Isolation
- Durability
- Can NoSQL Databases be ACID-compliant?
  
# About

SQL databases are ACID-compliant. ACID stands for Atomicity, Consistency, Isolation, and Durability. These form a set of properties that ensure database transactions are processed reliably.

# Atomicity

Ensures the transaction is either executed completely (succeeds) or not at all (fails).

# Consistency

The transaction undergoes a rollback (is reverted) unless all database rules are applied. In other words, data inserted into the table must conform to the database schema.

# Isolation

Transactions operate independently without interference from concurrent transactions. 

# Durability

After a transaction is committed, it persists even if the system experiences a crash immediately afterward. Unlike memory cache, in the event of a server failure, the data remains (persistent storage).

# Can NoSQL Databases be ACID-compliant?

Yes, they can. Some NoSQL databases have implemented features to ensure ACID compliance, either fully or partially. One example is [mongoDB](https://www.mongodb.com/basics/acid-transactions), which has support for [multi-document ACID transactions](https://www.mongodb.com/blog/post/mongodb-multi-document-acid-transactions-general-availability). Another example is DynamoDB, with the newly added support for [transactions](https://aws.amazon.com/blogs/aws/new-amazon-dynamodb-transactions/) that has enabled ACID properties to be enforced by the application.
