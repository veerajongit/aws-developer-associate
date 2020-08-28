# Scalability

## Vertical Scalability
- Increasing the size of the instance (Processor + RAM) [eg. Database - RDS, ElastiCache]
- Also called Scale up/ Scale down

## Horizontal Scalability
- Increasing the no. of instances/system for your application [eg. Distributed systems]
- High availability specially if system is configure across multiple AZs
- Alsa called Scale Out/Scale In


# Elastic Load Balancing (ELB)

## Load Balancer
- Load balancers are servers that forward internet traffice to multiple servers (EC2 Instances) downstream.
- Single point of access (DNS) to your application
- Seamlessly handle failures of downstream instances
- Perform regular health checks to your instances
- Provide SSL termination (HTTPs) for your websites
- Enforce stickiness with cookies
- High availability across AZs
- Seperate public and private traffics

## EC2 Load Balancer
- ELB is a managed load balancer
    - AWS gaurantees that it will be working
    - AWS takes care of upgrades, maintanence, high availability
    - AWS provides only a few configuration knobs

## Health Checks
- Health Checks are crucial for Load Balancers
- They enable the load balancer to know if instance it forwards traffice to are available to reply to requests
- The health check is done on port and a route (/heath is common)
- If the response is not 200 (OK). then the instance is unhealthy
- Happens in every 5 seconds (can manually configure timing)

## Types of Load Balancers on AWS
- **Classic Load Balancer** (v1 old gen) - 2009
    - HTTP, HTTPS, TCP
- **Application Load Balancer** (v2 - new gen) - 2016
    - HTTP, HTTPS, Web Sockets
    - Routing table to different target groups (EC2 instances, ECS tasks, Lambda functions)
        - Routing based on path in URL
        - Routing based on hostname in URL
        - Routing based on Query string in URL
    - Great for microservices and container based applications
    - The application servers don't see the IP of the client directly
        - The true IP of the client is inserted in the header X-Forwarded-For
        - We can also get Port (X-Forwarded-Port) and proto (X-Forwarded-Proto)
- **Network Load Balancer** (v2 - new gen) - 2017
    - TCP, TLS (secure TCP) & UDP
    - Allows to handle millions of request per seconds (Extreme performance)
    - Less lantency ~100ms (vs 400 ms for ALB)
    - Target group will be Instance or IP
    - NLB is not configured with Security Group, so you have to open TCP access of your instance to be accessible from anywhere
- Recommended to use newer Load Balancers
- Can setup internal (private) or external (public) load balancers

## Good to know
- LBs can scale but not instantaneously - contact AWS for a "warm-up"
- Troubleshooting
    - 4xx errors are client induces errors
    - 5xx errors are application induces errors
    - 503 means LB is at capabity or no registered target
    - If the LB can't connect to your application, check your SG
- Monitoring
    - ELB access logs will log all access requests
    - CloudWatch metrics will give you aggregate statistics

## Load Balancer Stickiness
- So that the same client is always redirected to the same instance behind a load balancer
- Works for Classic Load Balancers and Application Load Balancers
- The cookie used for stickiness has an expiration date you control
- Use case: make sure the user doesn't lose his session data
- Enabling stickiness may bring imbalance to the load over the backend EC2 instances

## Cross-Zone Load Balancing
- Evenly distribute traffice across all the registered instances across all AZs
- Otherwise, each load balancer node distributes requests evenly across registered instances in its AZs only
- Always on on ALB by default but by default disables in CLB and NLB

## ELB - Connection Draining
- Called connection draining for CLB and Deregistration delay for ALB and NLB
- Stops sending new requests to the instance which is de-registering
- Between 1 to 3600 seconds, default is 300 seconds
- Can be disabled (set value to 0)
- Set low if requests are short

# Auto Scaling Group (ASG)
