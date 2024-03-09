
Create a new CronJob named google-ping. When executed, the Job should run a curl command for google.com. Pick an appropriate image. The excution should occur every two minutes.

    kubectl create cronjob google-ping --image=nginx:latest --schedule="*/2 * * * *" -o yaml --dry-run=client  -- /bin/sh -c 'curl google.com'  > cronjob.yaml

    k apply  -f cronjob.yaml  -n ckad-6

    kubectl -n ckad-6 get cronjobs,jobs,pods
    +++

    NAME                        SCHEDULE      SUSPEND   ACTIVE   LAST SCHEDULE   AGE
    cronjob.batch/google-ping   */2 * * * *   False     0        105s            6m23s

    NAME                             COMPLETIONS   DURATION   AGE
    job.batch/counter                1/1           26s        24h
    job.batch/eddy                   1/1           19s        24h
    job.batch/google-ping-28499144   1/1           5s         5m45s
    job.batch/google-ping-28499146   1/1           5s         3m45s
    job.batch/google-ping-28499148   1/1           5s         105s
    job.batch/random-hash            5/5           13s        31m

    NAME                             READY   STATUS      RESTARTS   AGE
    pod/counter-kvfwb                0/1     Completed   0          24h
    pod/eddy-tvsdl                   0/1     Completed   0          24h
    pod/google-ping-28499144-xfjk9   0/1     Completed   0          5m45s
    pod/google-ping-28499146-jcl7g   0/1     Completed   0          3m45s
    pod/google-ping-28499148-8s4r5   0/1     Completed   0          105s
    pod/random-hash-9gvh6            0/1     Completed   0          31m
    pod/random-hash-jcnrc            0/1     Completed   0          31m

Tail the logs of the CronJob at runtime. Check the command-line options of the relevant command or consult the Kubernetes documentation.

    kubectl get cronjob -w  -n ckad-6

    NAME          SCHEDULE      SUSPEND   ACTIVE   LAST SCHEDULE   AGE
    google-ping   */2 * * * *   False     0        113s            8m31s
    google-ping   */2 * * * *   False     1        0s              8m38s
    google-ping   */2 * * * *   False     0        5s              8m43s
    google-ping   */2 * * * *   False     0        5s              8m43s

Reconfigure the CronJob to retain a history of seven executions.
    
    ....
    spec:
      successfulJobsHistoryLimit: 7
    ....
    k apply  -f cronjob2.yaml  -n ckad-6


Reconfigure the CronJob to disallow a new execution if the current execution is still running. Consult the Kubernetes documentation for more information.

    ....
    spec:
      concurrencyPolicy: Forbid
    ....

    k apply  -f cronjob3.yaml  -n ckad-6