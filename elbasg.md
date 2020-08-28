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
- The goal of an ASG is to
    - Scale out (add EC2 instances) to match an increased load
    - Scale in (remove EC2 instances) to match a decreased load
    - Ensure we have a minimum and a maximum no. of machines running
    - Automatically register new instances to a Load Balancer
- Attributes
    - A launch configuration
        - AMI + instance type
        - EC2 User data
        - EBS Volumes
        - Security Groups
        - SSH Key Pair
    - Min Size/ Max Size/ Initial Capacity
    - Network + Subnets Information
    - Load Balancer Information
    - Scaling Policies

## Auto Scaling Alarms
- It is possible to scale an ASG based on CloudWatch alarms
- An alarm monitors a metric (such as Average CPU)
- Metrics are computed for the overall ASG instances
- Based on the alarms can create scale out/scale in policies

## Auto Scaling New Rules
- Target Average CPU Usage
- No. of requests on the ELB per instance
- Average network in/ Average network out

## Auto Scaling Custom Metric
- We can auto scale based on a custom metric (eg no of connected users)
- Send custom metric from application on EC2 to CloudWatch (PutMetric API)
- Create CloudWatch alarm to react to low/high values
- Use the CloudWatch alarm as the scaling policy for ASG

## Scaling Policies
- Target Tracking Scaling
    - Most simple and easy to setup (eg I want the avg ASG CPU to stay at around 40%)
- Simple/ Step Scaling
    - When a CloudWatch alarm is triggered (eg CPU > 70%), then add 2 units
    - When a CloudWatch alarm is triggered (eg CPU < 30%>), then remove 1
- Scheduled Actions
    - Aniticipate a scaling based on known usage patterns
    - Eg. increase the min capacity to 10 at 5pm on Fridays

## Scaling cooldowns
- Cooldown period helps to ensure the your ASG doesn't launch or terminate additional instances before the previous scaling activity takes effect
- We can create cooldowns that apply to a specific simple scaling policy
- A scaling-specific cooldown period overrides the default cooldown period
