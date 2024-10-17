<div align='center'>
  <h1>Design a Big Data Pipeline for Batch and Real-Time Analytics</h1>
</div>

# Table of Contents

- [Functional Requirements](#functional-requirements)
- [Non-Functional Requirements](#non-functional-requirements)
- [Lambda Architecture (AWS Solution)](#lambda-architecture-aws-solution)
- [Lambda Architecture (Apache Spark Solution)](#lambda-architecture-apache-spark-solution)
- [References](#references)

# Functional Requirements

The system has to process a large amount of data with parallel (multi-thread) processing.

# Non-Functional Requirements

The system should be fault-tolerant, scalable, and database-agnostic.

# Lambda Architecture (AWS Solution)

- Batch layer: input raw data is stored in a data lake such as AWS S3.

- Speed layer: ...

- Serving layer: ...

# Lambda Architecture (Apache Spark Solution)

- Batch layer: in a Hadoop cluster, input raw data is stored in the Hadoop Distributed File System (HDFS). YARN is used for resource management and task scheduling within the cluster. Apache Spark processes the data by executing scripts written in languages such as Java, R, Scala, Python, and SQL, distributing tasks across the cluster to handle the data in a parallelized manner.

- Speed layer: ...

- Serving layer: ...

# References

[1] https://aws.amazon.com/blogs/big-data/build-a-big-data-lambda-architecture-for-batch-and-real-time-analytics-using-amazon-redshift/
