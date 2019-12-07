## Jobs
Used to reliably execute a workload until it completes.

The job will create one or more pods. When the job is finished, the containers will exit and the pods will enter the *Completed* state.

CronJobs allow you to execute jobs according to a schedule.
A CronJob's spec contains a schedule (as a *cron expression*). It also contains a jobTemplate to specify the job we want to run.
