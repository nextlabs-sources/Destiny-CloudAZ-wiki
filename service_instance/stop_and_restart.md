# Stop Running Service Instance and Restart

## Summary

In some occasion, there's request to stop the stack and restart it later. Since a running CloudAz stack contains many components, a proper approach must be followed to avoid data lose.

## Components can be stopped

In a CloudAz stack, following components can be stopped (or delete and restore with an identical new copy).

* Control Center
* Java Policy Controller
* RDS

> Load Balancers can't be stopped.

## Steps to stop the components

### Control Center

Control Center can be stopped directly by stopping the EC2 instance inside which it runs.

### Java Policy Controller

Java Policy Controllers are mortal, they are managed by Auto Scaling Group. Stop the Java Policy Controller means suspend the Auto Scaling Group and stop the EC2 instances.

1. First, stop the Java Policy Controller's Auto Scaling Groups' "ReplaceUnhealthy" process. Follow [AWS's document](http://docs.aws.amazon.com/autoscaling/latest/userguide/as-suspend-resume-processes.html) to do this.
2. Second, stop all the EC2 instances under the Java Policy Controller's Auto Scaling Groups.

### RDS

RDS can't be stopped, but can be restored properly with a snapshot.

1. Make a manual snapshot of the RDS instance.
2. Delete the running instance.

For detailed instruction, refer to section "Upgrade Control Center > Do snapshot before doing upgrade" under doc [CloudAz Service Patching and Upgrade](patching_upgrade.md).

## Steps to bring back (restart) the components

### RDS

For detailed instruction, refer to section "Restore the RDS and Control Center EC2 instance when upgrade failed > Steps to restore RDS" under doc [CloudAz Service Patching and Upgrade](patching_upgrade.md).

### Control Center

Just start the EC2 instance. If your Control Center Console is not accessible after server starts, probably the ControlCenterES service inside the EC2 linux instance is dead and locked, refer to [this doc](https://sharepoint.nextlabs.com/sites/yoda/kb/Lists/Posts/Post.aspx?ID=302 ) for troubleshoot steps.

### Java Policy Controller

1. Start all Java Policy Controller EC2 instances under the Auto Scaling Group.
2. Resume the Auto Scaling Group's "ReplaceUnhealthy" process.
