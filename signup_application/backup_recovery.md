## Backup and Restore

The data exist in the signup app falls in 2 pillars: database data and application logs.

### Database Data

Signup app is using MongoDB, an advised backup strategy would be:

* A cron job that dumps the whole db and save it to S3.
* Keep recent 1 month's backup in S3

If the system is currupted or hacked, we can restore the whole db from S3 backup. Based on the SLO/SLA, a backup interval can be varied.

MongoDB official website has a [tutorial](https://docs.mongodb.com/manual/tutorial/backup-and-restore-tools/) for using mongodump and mongorestore utilities to backup and restore a small deployment of MongodDB.

### Application Log

The signup app is running as a Docker container inside an EC2 instance, the logs produced can be dumped using `docker logs` command. Dump the log and store in an external location such as S3 is recommended.

We have plan to use CloudWatch to store logs and using tools like **CloudWatch Agent** or **Fluentd** to stream the logs generated by the app to CloudWatch.
