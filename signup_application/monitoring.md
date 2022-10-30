## Monitor

## Docker Container Stream to CloudWatch

To push the docker container's log to CloudWatch, follow steps below:

1. Configure the Instance Profile for the EC2 running Signup app to have permissions for CloudWatch log push. (Refer to [AWS CloudWatch Log](http://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/QuickStartEC2Instance.html) for details).
2. Modify the Launch Configuration's userdata to include log driver settings for docker, for example:
```yaml
runcmd:
...
- docker run -d -e 'MONGO_URL=mongodb://nextlabs:123next!@10.2.155.225/cloudaz'
    -e  ROOT_URL=https://staging.cloudaz.net
    --restart=always -e PORT=443 -p 443:443 
    -v /var/log/cloudaz_logs:/app/programs/server/assets/app/logs
    --name cloudaz
    --log-driver=awslogs
    --log-opt awslogs-region=us-west-2
    --log-opt awslogs-group=/cloudaz/signupapp/docker
    579292207088.dkr.ecr.us-west-2.amazonaws.com/platform-saas-onboarding:build-37
...
```

## Monitor General Guide

Monitor should be done for the system including:

1. External check whether the application is healty (HTTP request - Nagios )
2. EC2 instance health, ELB healthy count, autoscaling group events
3. ELB 4xx request and 5xx request
4. MongoDB disk, healthy (use external tools to check connection)