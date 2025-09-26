---
title: 'Cloud'
date: 2024-03-20
draft: false
description: 'Cloud Computing'
slug: 'cloud'
tags: ['Cloud Computing']
---

### What is Cloud Computing?

- On-Premise

  - Less scalability (less flexibility)
  - Server Storage (have to maintain and power it)
  - Less Data Security
  - Data Loss (data recovery can be hard)
  - Maintenance have to be done by the company itself
  - Less collaboration

- Cloud
  - More scalability (more flexibility)
  - Server Storage (have to maintain and power it)
  - Better Data Security
  - Data Loss (data recovery measures are there)
  - Maintenance
  - More collaboration (easier to share data in diff locations)

### About Cloud

- Delivery of on demand computing services on the internet
- Pay-as-you-Go (pay more, scale up / pay less, scale down)
- Cost efficient

#### Deployment Model

1. Public Cloud

- Like a bus
- Lower cost (pay only for resources used)
- Owned by Cloud Providers

2. Private Cloud

- Like owning a car
- Managed by single organization or third party
- Pay large amount up front (owned by the company)

3. Hybrid Cloud

- Using both
- Like in Federal Agencies (store personal data, share nonsensitive data on public cloud)

#### Service Model

1. IaaS

- Infrastructure
  - Access to basic computing infrastructure
  - Commonly used by IT Administrators
  - Storage or VMs, Networking, Servers
  - Pay for What You Use

2. PaaS

- Platforms
  - Platform and runtime environments provided for Dev, Testing, Managing Applications
  - Commonly Used By Software Developers
  - Don't have to acquire, manage, maintain related architecture
  - Only have to handle App and Data

3. SaaS

- Software
  - Involves hosting and managing software applications
  - Not owning any IT equipment
  - Commonly Used by End Customers

#### Popular Services

- AWS
  - Services over the Internet
  - IaaS is their main
  - Subscriptions are pay what you use
- Azure
- Google Cloud Platform

#### Lifecycle of Cloud Computing Solution

- Understand the requirements of the business (Define purpose)
- Choose a compute service that will provide the right support where you resize the compute capacity in the cloud to run application programs (Define the Hardware ex. EC2, Lambda, Elastic Container Service)
- Define Storage - Choose storage service for backup and archive data (S3, EFS, Glacier)
- Define Network - securely deliver data, videos, applications, etc (VPC, Route 53, Direct Connect)
- Define Security - user Auth, etc. (IAM, KMS, Cognito)
- Define Management Processes and Tools (CloudWatch, Auto scaling, CloudFormation)
- Testing Process - CodeStar, CodeBuild, CodePipeline (build, test, deploy code quickly)
- Analytics - Athena, EMR, CloudSearch

## Containers

- Docker, Cloud Foundary, Kubernetes, etc.
