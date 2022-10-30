## Control Center Instance Restore

### Summary

In case of server failure of Control Center instance, a restore using [previous backup](cc_backup.md) can help recover the instance. 

> In case of database data corrupt, this retore guide won't help, database need be restore first before apply this Control Center restore steps.

### Restore Steps

1. Keep a note of existing EC2 server's details (security groups, VPC, Subnets, key pair, instance type, IAM role,etc).
2. Stop the existing EC2 instance from EC2 management console.
3. Under EC2 Management console, navigate to `Images > AMIs`. Select the correct AMI for the Control Center, then click launch. Specify same settings for the new instance as the old one.
4. After the instance is ready, verify it's healthy by means including:
    * SSH into the instance and check service status (`CompliantEnterpriseServer` and `ControlCenterES` services)
    * Check `DCC.0.log` under `<install_home>/server/logs` folder
5. Under EC2 Management console, navigate to `LOAD BALANCING > Load Balancers`. Select the correct load balancer for the Control Center, click the `Instances` then add the newly restored instance by click `Edit Instances`.
6. Remove the old instance from load balancer.
7. Verify the HTTPS pages are able to open by going to customer's Policy console webpage (`https://<customer_hostname>/console`).
 