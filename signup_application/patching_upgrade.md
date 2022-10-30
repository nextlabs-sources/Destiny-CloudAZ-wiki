# CloudAz Signup Application Patching and Upgrade

## Patching Upgrade Process (Blue-green deployment)

No manually SSH into the instance should be done (which should be considered as bad practice), the EC2 instance provisioned by autoscaling group should be considered mostly immutable.

To upgrade the signup application, we use a strategy similar to Blue-green deployment.

The detail upgrade process can be:

1. Create an updated Launch Configuration with new version of signup app configuration (docker image, etc).
2. Change the autoscaling group to use the new Launch Configuration.
3. Chage MAX, desired capacity of the autoscaling group to 2.
4. Wait for autoscaling group to provision a new instance (with new launch configuration) and wait until its status becomes inService.
5. Change desired capacity of the autoscaling group to 1.
6. Detach the old instance or set it to StandBy.
7. Now only the new instance is serving the traffic, verify it's working properly.
    * If the new instance is working properly:
        - Delete the old instance and set MAX capacity of autoscaling group to 1.
    * If the new instance is not working properly, we need to rollback:
        - Modifiy autoscaling group to use old Launch Configuration
        - Re-attach the old instance or exit it from StandBy
        - Delete the new instance and set MAX capacity of autoscaling group to 1.
        - Delete the new Launch Configuration