<div align='center'>
  <h1>Design an End-To-End LLM Big Data Pipeline for Batch Training and Real-Time Inference</h1>
</div>

# Table of Contents

- [Functional Requirements](#functional-requirements)
- [Non-Functional Requirements](#non-functional-requirements)
- [Infrastructure](#infrastructure)
- [Services](#services)
- [Lambda Architecture (AWS Solution)](#lambda-architecture-aws-solution)
- [Lambda Architecture (Apache Spark Solution)](#lambda-architecture-apache-spark-solution)
- [References](#references)

# Functional Requirements

- The system should be able to preprocess a large amount of data with parallel (multi-thread) processing. 

- The system should be able to load a large model fast, as well as having high throughput and low-latency inference requests.

- Workflow:
    1. User visits https://yourcustomdomain.com
    2. Types a query in the text box and clicks "Submit."
    3. The frontend sends the query to API Gateway.
    4. API Gateway triggers a Lambda function (or forwards directly) to query the SageMaker endpoint.
    5. SageMaker processes the input and returns a response.
    6. The response is displayed on the webpage.

# Non-Functional Requirements

- The system's infrastructure should be able to scale with increasing data size and neural network parameters (weights).
- The system should be fault-tolerant and database-agnostic.

# Infrastructure 

1. `Scalability`: 

- S3 scales automatically. 
- SageMaker can be set to [autoscale](https://docs.aws.amazon.com/sagemaker/latest/dg/endpoint-auto-scaling.html) to automatically adjust the number of instances according to workload.

2. `Availability and Fault Tolerance`:

- For availability, package the model with TensorFlow Lite or TensorFlow Serving and deploy it with SageMaker. Use API Gateway to expose the model as a real-time endpoint and keep the model always ready for inference.
- For fault tolerance, save training checkpoints periodically to S3. If a training job is interrupted, it can resume from the latest checkpoint instead of restarting.

3. `Fast Loading Times and Minimal Latency:`

- Amazon SageMaker Inference now uses the [Fast Model Loader](https://aws.amazon.com/blogs/machine-learning/introducing-fast-model-loader-in-sagemaker-inference-accelerate-autoscaling-for-your-large-language-models-llms-part-1/) to enable faster model loading while reducing inference time.

4. `Backend API:`

- The API for client-server communication can be either a [RESTful API](https://github.com/camponogaraviera/full-stack-roadmap/blob/dev/system_design_and_infrastructure/backend/restfull_api.md) (using [AWS API Gateway](https://aws.amazon.com/api-gateway/)+[Lambda](https://aws.amazon.com/lambda/)), a [GraphQL API](https://github.com/camponogaraviera/full-stack-roadmap/blob/dev/backend/grahql.md) (using [AppSync](https://aws.amazon.com/appsync/) + [AWS Amplify](https://aws.amazon.com/amplify/)), or [gRPC](https://github.com/camponogaraviera/full-stack-roadmap/blob/dev/backend/gRPC.md).

# Services

- Storage: use [AWS S3]() to store the training dataset and model metadata.

- Frontend Logic: manages the UX/UI. Can be implemented with React.js.

- Frontend Hosting: deploy and host the frontend (static part) with built-in SSL/TLS certificate support using [AWS Amplify](). 

- Backend logic: build a RESTful API with [AWS Lambda]() + [API Gateway]() to allow CRUD operations. Use Lambda to define the backend logic that will respond to events such as API requests passing through the API Gateway. 

- Backend Hosting: use custom backend hosted on EC2 or Beanstalk to handle complex workflows if needed.

- AI Model Hosting: deploy the trained model to an endpoint using [AWS Sagemaker hosting services]().

Ps: The backend infrastructure (e.g., SageMaker endpoints, API Gateway, and Lambda functions) is hosted in the AWS Region of your choice. One need to choose EC2 instance types when creating SageMaker endpoints.

# Lambda Architecture (AWS Solution)

- Speed layer: handles real-time data ingestion and updates such as user interaction and feedbacks for model fine-tuning. [Amazon Redshift Cluster]() can be used for analytics on data streams. 

- Batch layer: used for preprocessing of large datasets and model training. [Amazon Kinesis Data Firehose]() can batch, compress, transform, and encrypt input raw data streams before loading them into a data lake such as AWS S3.

- Serving layer: queries data from both the batch and speed layers to serve predictions or aggregated outputs. [Amazon Redshift Serverless]() can be used for querying. Other consumption services include [AWS SageMaker]() and [AWS QuickSight]().

# Lambda Architecture (Apache Spark Solution)

- Speed layer: ...

- Batch layer: in a Hadoop cluster, input raw data is stored in the Hadoop Distributed File System (HDFS). YARN is used for resource management and task scheduling within the cluster. Apache Spark processes the data by executing scripts written in languages such as Java, R, Scala, Python, and SQL, distributing tasks across the cluster to handle the data in a parallelized manner.

- Serving layer: ...

# References

[1] https://aws.amazon.com/blogs/big-data/build-a-big-data-lambda-architecture-for-batch-and-real-time-analytics-using-amazon-redshift/
