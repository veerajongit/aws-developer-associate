# AWS Route 53
- Managed DNS
- Route 53 is a global service
- DNS is a collection of rules and records which helps clients understand how to reach a server through its domain name
- Most common records are
    - A: hostname to IPv4
    - AAAA: hostname to IPv6
    - CNAME: hostname to hostname (works only for subdomains)
    - Alias: hostname to AWS resource (Free of charge + Health checks)
- Route 53 can use
    - public domain names you own (or buy)
    - private domain names that can be resolved by your instance in your VPCs
- Route 53 has advanced features such as 
    - Load balancing (through DNS - also called client load balancing)
    - Health checks
    - Routing policy: simple, failover, goelocation, latency, weighted, multi value


## Routing Policy: Simple
- Can add 2 A records, the client can choose which record to use to visit the site
- Used for client side load balancing

## Routing Policy: Weighted
- Control the % of the request that go to specific endpoint

## Routing Policy: Latancy
- Redirect to the server that has the least latency close to us
- Super helpful when latency of users is a priority
- Latency is evaluated in terms of user to designated AWS Region
- Germany may be directed to the US (if that's the lowest latency)

## Health checks
- Have X health checks failed => unhealthy (default 3)
- After X health checks passed => health (default 3)
- Default Health Check Interval: 30s (can set to 10s - higher cost)
- About 15 health checkers will check the endpoint health
- One request every 2 seconds on average
- can have HTTP, TCP and HTTPS health checks (no SSL verification)
- Possibility of integrating the health check with CloudWatch
- Health checks can be linked to Route53 DNS queries

## Routing Policy: Failover
- If health check of primary instance fails, route 53 will failover to the secondary instance

## Routing Policy: Geolocation
- Different from latency based
- Based on user location
- Eg. traffic should go to this specific IP
- Should create a "default" policy (in case there is no match on location)

## Routing Policy: Multi Value
- Routing traffice to multiple resource
- Want to associate a Route 53 health checks with records
- Up to 8 healthy records are returned for each Multi Value query
- Multi Value is not a substitute for having an ELB