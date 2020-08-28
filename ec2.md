# Elastic Compute Cloud (EC2)
It mainly consists of
- Renting virtual machine
- Storing data of virtual devices (EBS)
- Distributing load across machine (ELB)
- Scaling the services using an auto-scaling group (ASG)

## Connecting to your instance
- A standalone SSH client
- EC2 instance connect (browser based, only Amazon Linux 2 AMI)
- Java SSH client (browser based, require java)

Only the conenction needs inbound rule to allow port 22 for ssh connection

## Security groups
- Fundamental to network secuting in AWS
- They control how traffic is allowed into or out of EC2 machines
- Most fundamental skill to learn to troubleshoot networking issues
- Firewalls on EC2 instance
- Can authorize IP ranges - IPv4 and IPv6
- Can be attached to multiple instances
- An instance can have multiple Security Group
- Locked down to a region/VPC combination
- Good to maintain one seperate security group for SSH access
- If application is not accessible its probably security group issue, if the application gives a "connection refused" error, then it's an application error or it's not launched.
- All inbound traffic is blocked by default and all outbound traffic is authorised
- If correct Security group is attached to two or more instance they can directly communicate with each other via that Security group
- AWS provides only 5 Elastic IPs in your account(you can ask AWS to increase that)
- Try to avoid using Elastic IP
  - They often reflect poor architectural decisions.
  - Instead, use a random public IP and register a DNS name to it
  - Or use a load balancer and don't use public IP (best pattern)
- Public IP changes if the machine is stopped and restarted

## EC2 User Data
- It is possible to bootstrap our instance using an EC2 user data script
- Launches command when the machine start
- Only run once at the first start of the instance
- Any user data script will have sudo rights

## EC2 Instance launch types
- On demand instance: short workload, predictable pricing (Highest cost)
- Reserved instance: min. (1 year) 
  - Reserved Instances : long workloads
  - Convertible reserved instance: long workloads, with flexible instances
  - Scheduled reserved instance: eg Every thursday between 3-6 pm
- Spot instance: short workloads, for cheap, can lose instance (less reliable)
- Dedicated instance: no other customer will share your hardware
- Dedicated Host: book an entire physical server, control instance placement

## Elastic Network Interfaces (ENI)
- Logical component in a VPC that represents a virtual network card
- Attributes
  - Primary private IPv4, one or more secondary IPv4
  - One Elastic IP (IPv4) per private IPv4
  - One Public IPv4
  - One or more security groups
  - A MAC address
 - Can create ENIs independently and attach them on the fly on EC2 instances for failover
 - Bound to a specific AZ
 