<div align='center'>
  <h1> Servers </h1>
</div>

# Table of Contents

- Stateful vs Stateless 
- Cold standby
- Warm standby
- Hot standby 

# Stateful vs Stateless 

- Stateful: requests from the same client are routed to the same server.
- Stateless: future requests do not depend on data stored from previous requests, i.e., there is no response history between requests. There's no information about which server has served which request, i.e., there is no information about where the request was routed to. Any server can handle any request.

Horizontal scaling works best if the web servers are stateless.

# Cold standby

Data from master is copied to a standby slave. The process can take days if the master database is too large.

# Warm standby

Data from master is replicated constantly to the standby slave.

# Hot standby

There is no replication. Data is written simultaneously to the master and the standby slave. This is close to a horizontal scaling solution since it's also possible to read simultaneously.
