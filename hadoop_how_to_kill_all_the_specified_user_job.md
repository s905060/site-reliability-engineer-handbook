# Hadoop How to Kill all the specified user Job

In fact, achieve kill the specified user job is very simple, hadoop job command itself already carries a lot of useful job management function.

List all the jobs on Jobtracer

`hadoop job - List`

Use hadoop job -kill kill jobid specified

`hadoop job - kill job_id`

Combinations of the above two commands can be achieved kill off the specified user's job

```
for i in  `hadoop job -list | grep -w username | awk '{Print $ 1}' | grep job_` ;  do hadoop job - kill $ i ;  DONE
```