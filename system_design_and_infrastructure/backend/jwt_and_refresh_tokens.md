<div align="center">
  <h1> JWT and Refresh Tokens </h1>
</div>

# Table of Contents

- JWT
- Refresh Tokens
- Connection with REST APIs
- Connection with AWS Cognito and IAM

# JWT

- JWT is a compact, URL-safe way of representing claims to be transferred between two parties. It is commonly used for authentication and authorization in web applications.

- A JWT contains three parts: 
  - Header:
  - Payload: contains the user's data (claims).
  - Signature: verifies the token's integrity.

# Refresh Tokens

- A refresh token is a long-lived token issued alongside a short-lived JWT.

- While JWTs are used to access protected resources, refresh tokens are used to obtain new JWTs when the old ones expire. This allows users to stay logged in without re-entering credentials frequently.

# Connection with REST APIs

- In a REST API, JWTs are used for stateless authentication. A client includes the JWT in the Authorization header for each request to access secured endpoints.

- When the JWT expires, the client can send the refresh token to a designated endpoint to obtain a new JWT.

# Connection with AWS Cognito and IAM

- When a user signs in through AWS Cognito, it issues a JWT (ID token, access token, and optionally a refresh token).
  - The ID token contains information about the authenticated user (e.g., username, email) and is used for client-side operations.
  - The access token is used to authorize access to protected APIs and AWS services (e.g., API Gateway).
  - The refresh token is used to obtain new tokens without requiring the user to log in again.

- Use IAM to define policies that restrict access based on JWT claims without writing custom token-handling logic.
