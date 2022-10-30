## Deployment Diagram

!["Architecture Diagram"](signup_architecture.png)

## Components

The signup app is a nodejs web app built as a Docker image, a mongo db is required, the deployment in AWS including:

1. An MongoDB instance (a single EC2 instance or a cluster of EC2 instances)
2. An application load balancer (elbv2) for serving HTTP(S) traffice to the sign up app
3. 2 elb listeners for HTTP and HTTPS traffic respectively
4. 2 autoscaling target groups for HTTP and HTTPS traffic respectively
5. A valid HTTPS certificate for the domain to be used by sign up app (use Amazon ACM or IAM server certificate)
6. An autoscaling launch configuration to provision signup app
7. An autoscaling group with the 2 target groups to do actual instance deployment
8. Required security groups for different components

### Application Credentials

#### JWT (Json Web Token) Key Pair

The nodejs web app need to have a production key pair for generating and verifying REST API Tokens. The launch configuration of sign up app should including steps to pull the key pair (for example, from a secure S3 location) and deploy the key pair.

Refer to [destiny-cloudaz-portal doc](https://bitbucket.org/nxtlbs-devops/destiny-cloudaz-portal/src/master/docs/jwt_key.md?at=master&fileviewer=file-view-default) for steps to generate the key pair.

### Lambda Functions

Few lambda functions are required to deployed together with the signup app:

* L1. [signup_marketplace_instance_provisioner](https://bitbucket.org/nxtlbs-devops/devops-aws-scripts/src/master/CloudAz-SAAS/signup_marketplace_instance_provisioner/)

### Auto Scaling Group

The autoscaling group is not used to scale out the application, it's used to:

* Make sure there's always one running instance of the signup app
* Facilitates deployment 

The components are already set up properly in staging and production, a normal application upgrade doesn't require to re-create them. The main component that need to be changed would be Launch Configuration.
