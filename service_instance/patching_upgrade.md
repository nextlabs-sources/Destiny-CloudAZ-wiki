# CloudAz Stack Upgrade and Patching Guide

## Summary

This doc provides instruction to upgrade a CloudAz Stack (AWS CloudFormation stack) to newer version which includes:

* Upgrading Control Center Server
* Upgrading Java Policy Controller Server
* General Guide for doing patching

> The guide is based on both 8.0.4 and 8.0.6 release of CloudAz artifacts and CloudFormation templates. Future release of CloudAz may be changed to use different creation/deployment strategy, refer to newer docs for newer releases.

## Upgrade Control Center

Control Center runs inside an EC2 instance in one CloudAz Stack. General information about the set up:

* The EC2 instance is running Amazon Linux and created using official Amazon AMI
* Control Center is provisioned with cloud-init (by specifying user-data)
* Control Center is installed under `/opt/nextlabs` folder.
* Control Center's Linux service name is `CompliantEnterpriseServer`.

### Do snapshot before doing upgrade

It's safer to create one snapshot of database and one snapshot of Control Center's EBS volume before doing any upgrade.

To create a manual snapshot of the RDS.

1. In RDS Management Console, navigate to left side panel's `Snapshots` section.
2. Create a snapshot by choosing the RDS instance and a recognizeble name of the snapshot.

To create a snapshot of existing Control Center's EBS volume:

1. In EC2 Management Console, at left side panel's `Instances` section, find the Control Center instance.
2. In the description section of the instance, find the `Root Device`'s EBS ID.
3. Navigate to the left side panel's `Snapshots` section, create a snaphost by specifying the EBS ID and a recognizable name.

### Steps to upgrade an existing Control Center

1. SSH into the EC2 instance running Control Center.
2. Download newer version of Control Center Linux SAAS version installer into a temporary folder (for example: /tmp). Assume the installer is downloaded at `/tmp/ControlCenter-Linux-chef-SaaS-x.x.x-Main.zip`
3. Unzip the installer. Assume installer is unziped under `/tmp/PolicyServer`
4. Modify property file of installer:
    Under installer folder, edit the file cc_properties.json and replace its content to:
```json
{
    "installation_mode": "upgrade"
}
```
5. Stop existing Control Center:
```bash
sudo service CompliantEnterpriseServer stop
```
6. Run the installer:
```bash
# cd into the installer folder
chmod +x ./install.sh
sudo ./install.sh -s
```
7. If everything goes well, the Control Center will be upgraded.
8. If any plugins need to upgrade or re-configure, follow the documentation for the plugin.
9. Start the server:
```bash
sudo service CompliantEnterpriseServer start
```
10. Clean up: Remove the installer after upgrade.

## Restore the RDS and Control Center EC2 instance when upgrade failed

If the upgrade failed, using the snapshot created, we can restore the server.

### If the upgrade failed but Database is not touched

In some circumstances, upgrade process has not changed database (not reaching database migration part). Using the EBS snapshot can restore the Control Center.

### If the upgrade failed and Dabase is changed

If the upgrade process already modified database (table schema, etc), restoring RDS is also required.

### Steps to restore RDS

>NOTE: It's recommended to stop servers that connecting to the RDS before doing the process.

1. Navigate to RDS Management Console
2. Note down following settings for the existing RDS instance:
    * Multi-AZ Deployment: yes or no
    * DB Instance Identifier: when restore the RDS, must use same identifier
    * DB Subnet Group
    * DB Security Group
3. Delete existing RDS.
4. Navigate to left side panel's `Snapshots` section, choose the created snapshot.
5. Click `Snapshot Actions` -> `Restore Snapshot`.
6. Specify details of the RDS, use default values already populated by the UI and specify the same value for `Mutli-AZ Deployment`, `DB Instance Identifier`, `DB Subnet Group` and `DB Security Group`.
7. Wait for the RDS to be created.

### Steps to restore EC2 instance

To restore EC2 instance, no need to delete the EC2 instance. Detail steps would be:

1. Create a new EBS volume from the snapshot, specify same available zone as existing Control Center EC2's instance.
2. Stop the Control Center EC2 instance.
2. Detach the original root volume, note down the device name (`/dev/xvda`)
3. Attach the new EBS volume to the EC2 instance with same device name.
4. Start the instance.

## Upgrading Java Policy Controller

Upgrading Java Policy Controller would be different from upgrading Control Center since Java Policy Controller is managed by a AWS Auto Scaling Group.

Overall steps to upgrade Java Policy Controller is:

1. Create a new Launch Configuration based on existing one with updated `user-data`.
2. Modify the Auto Scaling Gruop to use the new Launch Configuration.
3. Double the Auto Scaling Group capacity for the stack to let Auto Scaling Group create same number of new Java Policy Controllers with new Launch Configuration.
4. Verify the new instances works okay.
5. Modify the Auto Scaling Group capacity back to its original value (half) and Auto Scaling Group will terminate old instances. (According to AWS document, when scaling down, AutoScaling will first terminate instances with an old launch configuration, and if they all have the same configuration, then it will terminate the oldest instances first.)
6. Delete old Launch Configuration.

For how to create a new Launch Configuration with updated `user-data`, it depends on feature change of Java Policy Controller, the developers should provide doc for the change from one specific version to another one.

## General Guide for Doing Patching

A patching can be a single file replacement like a war file or certificate file change or a configuration file modification. It's better to create full snapshot before doing patching, but depends on the situation it may not required if the patching is small and safe.