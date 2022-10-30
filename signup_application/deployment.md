## Current Deployment

- Staging
  - AWS account: engservices
  - Region: us-west-2
  - Domain: staging.cloudaz.net
- Production
  - AWS account: CloudAz saas account
  - Region: us-east-1
  - Domain: www.cloudaz.com

## Deployment Process

### EC2 Deployment Process

> Choose a VPC with at least 2 public subnets and 1 private subnets (with NAT gateway so that instance can connect to Internet).

> Prerequisite: An instance profile with a role having proper IAM permissions for the Launch Configuration.

> The action items listed below is only applicable when creating all the things in one AWS region from scratch.

1. Create security groups for MongoDB instance or cluster (let's call it mongo-sg)
2. Provision a MongoDB instance or cluster in the private subnets with the security group created for it, enable password authentication
3. Create security group for signup app (let's call it signup-sg)
4. Create security group for signup app's load balancer (let's call it signup-elb-sg)
5. Create Launch Configuration for signup app's autoscaling group
6. Create 2 target groups for signup app for HTTP and HTTPS traffic respectively
7. Create an application load balancer for signup app
8. Create an ACM HTTPS certificate if not exisit for the domain to be used for sign up app
8. Create 2 load balancer listeners for HTTP and HTTPS traffic respectively, attach the HTTP target group to the HTTP (80) listener and attach the HTTPS target group to the HTTPS (443) listener, configure the HTTPS listener to use correct ACM HTTPS certificate.
9. Create an auto scaling group with the 2 target groups and the launch configuration, set MIN, MAX, desired capacity all as 1.

Detailed scripts and document to do the whole deployment is in bitbucket project [devop-aws-scripts](https://bitbucket.org/nxtlbs-devops/devops-aws-scripts) under path [CloudAz-SAAS/signup-app-deploy-scripts](https://bitbucket.org/nxtlbs-devops/devops-aws-scripts/src/master/CloudAz-SAAS/signup-app-deploy-scripts/?at=master).

### Lambda Deployment Process

The Signup application works with 2 Lambda functions.

#### Lambda function `signup_marketplace_instance_provisioner`

Refer to [signup_marketplace_instance_provisioner Readme](https://bitbucket.org/nxtlbs-devops/devops-aws-scripts/src/master/CloudAz-SAAS/signup_marketplace_instance_provisioner/?at=master) for details.
