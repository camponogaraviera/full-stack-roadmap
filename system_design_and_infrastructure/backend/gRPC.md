<div align='center'>
  <h1>gRPC</h1>
</div>

# Table of Contents

- About
- Best suited for
- Advantages over RESTful APIs

# About

gRPC is a high-performance Remote Procedure Call framework developed by Google to build scalable and fast APIs.	

# Best suited for

Suitable for applications that:

1. Require real-time bi-directional communication/streaming;
2. Work across different environments (NodeJs, Java, Golang, C#);
3. Have high-performance (low latency and high throughput) and scalability.

# Advantages over RESTful APIs

1. gRPC uses Protocol Buffers for serialization of structured data in a forward and backward-compatible way | REST uses JSON for data exchange.
2. gRPC uses HTTP 2 | REST uses HTTP 1.
3. gRPC supports data streaming (unary, server, client, bidirectional) | REST supports only unary streaming.
4. gRPC does not enforce a strict design schema | REST enforce strict schemas.

Types of data streaming APIs:

1. Unary (single request, single response).
2. Server streaming (single request, multiple responses).
3. Client streaming (multiple requests, single response).
4. Bidirectional streaming (multiple requests, multiple responses).
