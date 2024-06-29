<div align='center'>
  <h1>GraphQL</h1>
</div>

# Table of Contents

- About
- Best suited for
- CRUD Operations in GraphQL
- Similarities with RESTful APIs
- Advantages over RESTful APIs

# About

GraphQL is a schema-based API query language that enables client-server communication typically operating over a single endpoint using HTTP.

- Pros: GraphQL can be used in both the frontend and the backend of an application.
  - Backend: in the backend of the application, GraphQL is used to define a schema that specifies the structure of the data and the available operations (queries and mutations). Alongside the schema, resolvers (functions) are implemented to handle the aforementioned operations such as fetching the data from the appropriate sources and shaping it according to the schema before returning it to the user.
  - Frontend: in the frontend, GraphQL can be used alongside [Apollo Client](https://github.com/camponogaraviera/full-stack-roadmap/blob/dev/system_design_and_infrastructure/frontend/apollo_vs_redux.md) to handle the interaction between the client-side (frontend) and the server-side (backend). For example, Apollo Client can be used to send GraphQL [queries](https://www.apollographql.com/docs/react/data/queries/) and [mutations](https://www.apollographql.com/docs/react/data/mutations/) to the backend server.
  
- Cons: it requires server-side implementation to support GraphQL in the frontend.

# Best suited for

A GraphQL-based API is best suitable for:
1. Large, complex, and interrelated data.
2. For data access through a single URL endpoint.
3. When there is limited bandwidth and a need to limit the number of requests and responses.
4. When there is a need to combine multiple data sources into a single endpoint.
5. When Client requests and responses are expected to vary.

# CRUD Operations in GraphQL

CRUD operations are those that can be used to access and manipulate a resource (data or object). There are four such operations:

- Mutation (Create): used to create a new resource by sending a post request to a specific endpoint.
- Query (Read): used to retrieve a resource (read-only) from the server by sending a query request to a specific endpoint.
- Mutation (Update): used to update a resource by sending a put request to a specific endpoint.
- Mutation (Delete): used to delete a resource by sending a delete request to a specific endpoint.

# Similarities with RESTful APIs

1. Both are stateless: future requests do not depend on data stored from previous requests, i.e., there is no response history between requests. There's no information about which server has served which request, i.e., there is no information about where the request was routed to. Any server can handle any request.
2. Both use a Client-Server model: requests from a single client result in responses from a single server.
3. Both are HTTP-based: use CRUD operations to create, read, update, and delete data.
4. Both use JSON, XML, or HTML data formats to return data from the server to the client.
5. Both support caching.
6. Both are database agnostic, i.e., they can work with `any database structure` and programming language.

# Advantages over RESTful APIs

Problems in RESTful APIs:
1. Under-fetching: the response returns a whole data item (e.g., person's name, date of birth, address, and phone number) when only requesting for specific data (e.g., phone number).
2. Overfetching: when multiple REST API requests are needed to gather related data (e.g., a user's phone number and last purchase) from different endpoints (/person and /purchase).

GraphQL solution:
1. uses a single API request call to fetch/retrieve the exact data needed and no more.
