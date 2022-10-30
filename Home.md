# CloudAz Deployment Wiki

This wiki uses the [Markdown](http://daringfireball.net/projects/markdown/) syntax. The [MarkDownDemo tutorial](https://bitbucket.org/tutorials/markdowndemo) shows how various elements are rendered.[Bitbucket documentation](https://confluence.atlassian.com/display/BITBUCKET/Use+a+wiki) has more information about how using a wiki.

The wiki itself is actually a git repository, which means you can clone it, edit it locally/offline, add images or any other file type, and push it back to us. It will be live immediately.

Go ahead and try:

```
$ git clone https://bitbucket.org/nxtlbs-devops/cloudaz-wiki.git/wiki
```

## CloudAz Signup Application

[CloudAz Signup Application](https://bitbucket.org/nxtlbs-devops/destiny-cloudaz-portal) is a nodejs application using mongoDB to allow users signup CloudAz instances.

The deployment of Signup Application is done on AWS and it works together with few Lambda functions which also need to be deployed.

### Architecture

[CloudAz Signup Architecture](signup_application/aws_architecture.md)

### Common Tasks

#### Installation (deployment)

[Cloudaz Signup Deployment](signup_application/deployment.md)

#### Patching and Upgrade

[Cloudaz Signup Patching and Upgrade](signup_application/patching_upgrade.md)

#### Monitoring

[CloudAz Signup Monitoring](signup_application/monitoring.md)

#### How to collect Data

### Advanced Tasks

#### Backup and Recovery

[CloudAz Signup Backup and Recovery](signup_application/backup_recovery.md)

##### Backup Procedure
##### Recovery Procedure

### Miscellaneous

[CloudAz Signup Miscellaneous](signup_application/miscellaneous.md)

### FAQ / Tips

## CloudAz Service Instance

CloudAz Service is a per instance per tenant service. A single CloudAz service instance means a stack of resources allocated for a single customer.

On AWS, CloudFormation is used to create a CloudAz service instance.

### Architecture

[CloudAz Service Architecture](service_instance/aws_architecture.md)

### Common Tasks

### Changes Tracking

## Major Changes
Customized AWS AMI for CC and JPC:
https://sharepoint.nextlabs.com/sites/dpaas/SitePages/Topic.aspx?RootFolder=%2Fsites%2Fdpaas%2FLists%2FCommunity%20Discussion%2FCustumized%20AMI%20for%20CC%20and%20JPC&FolderCTID=0x012002003EC3F4F2D85EB2408701ECBCFF05F9A2&SiteMapTitle=CloudAz&SiteMapUrl=https%3A%2F%2Fsharepoint%2Enextlabs%2Ecom%2Fsites%2Fdpaas%2FSitePages%2FCategory%2Easpx%3FCategoryID%3D2%26SiteMapTitle%3DCloudAz

## To request any changes, please go to:
https://nextlabs.sharepoint.com/sites/cm/Lists/Change%20Management%20Request/AllItems.aspx


#### Installation (deployment)
#### Patching and Upgrade

[CloudAz Service Patching and Upgrade](service_instance/patching_upgrade.md)

#### Monitoring
#### How to access EC2 instances

### Advanced Tasks

#### Backup Procedure

[Control Center Instance Backup](service_instance/cc_backup.md)

#### Recovery Procedure

[Control Center Instance Restore](service_instance/cc_restore.md)

### Miscellaneous

[Stop Running Service Instance and Restart](service_instance/stop_and_restart.md)

### FAQ / Tips