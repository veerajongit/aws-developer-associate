# Fundamentals

## AWS Regions
AWS has many datacenters all around the world, they are called regions.
Regions are a set of data centers. A region is a cluster of data centers. AWS services are region-scoped except IAM.

## AWS Availabilty zones
Each region has many availability zones - AZ (usually 3, min 2, max 6).
Each AZ is one or more discrete data centers with redundant power, networking and connectivity. They are seperate from each other so that they are isolated from disasters
They're inter-connected with high bandwidth, ultra-low latency networking.

Pick a region closer to you so that you get fast access.

# IAM
Identity and access management for Users, Groups and Roles.
Root account should not be used (and shared). Users must be created with proper permissions. IAM is at the center of AWS. There are policies within IAM which are written in JSON (JavaScript Object Notation). 

Users - Physical person

Groups - Users can be grouped together (By funtions - admin, devops, etc or By teams - engineering, design, etc)

Roles - Internal usage within AWS resources

Policies - JSON documents which defines what users, groups and roles can and cannot do.

IAM has global view. Multifactor Authentication (MFA) can be configured. IAM has predefined "managed policies". Its best to give users the minimal amount of permissions they need to perform their job (least privilege principles). Big enterprise can use IAM federation, the users can use this to login to AWS using their company credentials.

One IAM User per Physical Person

One IAM Role per Application

IAM credentials should Never be shared

Never write IAM credentials in code

Never use the Root account except for initial setup

Never use Root IAM credentials