
Linux implements task scheduling through a utility called Cron. 

Cron is a time-based service that runs applications, scripts and other commands repeatedly on a specified schedule. 

An application, or script that has been configured to be run repeatedly with Cron is known as a Cron job. Cron can be used to automate or repeat a wide variety of functions on a system, from daily backups to system upgrades and patches. 

The crontab file is a configuration file that is used by the Cron utility to store and track Cron jobs that have been created.


## **Exploiting Misconfigured Cron Jobs

Cron jobs can also be run as any user on the system, this is a very important factor to keep an eye on as we will be targeting Cron jobs that have been configured to be run as the “root” user. 

This is primarily because, any script or command that is run by a Cron job will run as the root user and will consequently provide us with root access.

In order to elevate our privileges, we will need to find and identify cron jobs scheduled by the root user or the files being processed by the cron job.