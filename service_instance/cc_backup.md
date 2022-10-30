## Control Center Instance Backup

### Summary

Control Center Server runs inside an EC2 instance. It connects to RDS and maps an EFS volume to it's path `/mnt/efs` to store configuration files (static properties files, json files etc) and certificates. Backing up Control Center requires to have all the server components including:

* OS
* Installed server binaries (including service files)

The backup of Control Center doesn't including:

* Database data
* EFS data
* logqueue data (under `<install_home>/server/logqueue`)
* Control Center log files (under `<install_home>/server/logs`)

### Backup scenarios

The proposed backup strategy is making a machine image (AMI) of the EC2 instance running Control Center.

1. Before hand over to customer

The service instance is provisioned by automated scripts (lambda functions). After the instance is ready, before share the instance details to customer (via sign up app backend), a backup of the healthy Control Center is necessary in case of server failure later.

2. After Control Center upgrade

After the server is upgraded, a new backup of the healthy instance should be created also. Before the upgrade, please follow the [patching and upgrade guide](patching_upgrade.md) of Control Center for any backup related thing.

### Backup Steps

To create a Control Center machine image (AMI), follow the [AWS AMI Guide](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/creating-an-ami-ebs.html). In summary, the steps would be:

1. Under EC2 management console, navigate to the EC2 instance running Control Center.
2. Right click the instance, click option `image > Create Image`.
3. Follow the UI guide and give the image a proper name that can be recognized when doing restore work.
