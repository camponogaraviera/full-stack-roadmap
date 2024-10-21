<div align='center'>
  <h1> URL & DNS </h1>
</div>

# Table of Contents

- URL
- DNS
- Registering a DNS with AWS

# URL

URL structure: `protocol://domain.TLD:port/path?query_string#fragment_identifier`.

Meaning:
- `Protocol`: could be FTP (File Transfer Protocol), SMTP (Simple Mail Transfer Protocol), HTTP (HyperText Transfer Protocol), or HTTPS (HyperText Transfer Protocol Secure).
- `Domain`: is the domain name of the server hosting the application. This domain name gets resolved to an IP address via DNS.
- `TLD`: The top-level domain (.com, .net, .org, etc.).
- `Port (optional)`: specifies the connection endpoint on the server where the client should connect. It is an optional parameter and if not specified, defaults to a well-known port for the given protocol (e.g., 80 for HTTP, 443 for HTTPS, or 3000 for a Node.js express server).
- `Path (optional)`: specifies the specific resource or endpoint to be served.
- `query_string (optional)`: contains the parameters for a query to be applied to the resource. Usually in the form of key-value pairs.
- `fragment_identifier (optional)`: identifies a specific location/section within the resource by its ID or name attribute. Works as a bookmark/anchor point for fast access within the page document.

# DNS

A DNS (domain name system) is a naming database that works as a mapping, translating human-friendly domain names (example.com) into IP addresses (e.g. 192.0.2.1).

A single DNS can be associated with multiple IP addresses. This setup helps distribute traffic load across multiple servers, preventing overload on a single server (when there are too many requests to a single server). This technique is often referred to as load balancing or sharding (more on that in the following chapters). This allows millions of users to access the same website at the same time from different locations. Many system designs use GeoDNS for traffic redirection to find the closest server to the client.

One can search for the ownership of a domain at https://www.whois.com/whois/.

Official Domain Name Registrars:

- US: https://www.enom.com/ 
- BR: https://registro.br/

# Registering a DNS with AWS

It is possible to register a new domain using [Amazon Route 53](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/domain-register.html).
