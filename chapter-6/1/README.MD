Create a Job named random-hash using the container image alpine:3.17.3 that executes the shell command echo $RANDOM | base64 | head -c 20. Configure the Job to execute with two Pods in parallel. The number of completions should be set to five.

    kubectl create job random-hash --image=alpine:3.17.3 \
    -o yaml --dry-run=client \ 
    --  /bin/sh -c 'echo $RANDOM | base64 | head -c 20'  > random-hash.yaml

    k apply -f random-hash.yaml  -n ckad-6



Identify the Pods that executed the shell command. How many Pods do expect to exist?

    kubectl -n ckad-6 get jobs,pods

    NAME                    COMPLETIONS   DURATION   AGE
    job.batch/counter       1/1           26s        24h
    job.batch/eddy          1/1           19s        23h
    job.batch/random-hash   5/5           13s        22s

    NAME                    READY   STATUS      RESTARTS   AGE
    pod/counter-kvfwb       0/1     Completed   0          24h
    pod/eddy-tvsdl          0/1     Completed   0          23h
    pod/random-hash-9gvh6   0/1     Completed   0          13s
    pod/random-hash-jcnrc   0/1     Completed   0          17s
    pod/random-hash-lnc89   0/1     Completed   0          22s
    pod/random-hash-qqwk5   0/1     Completed   0          22s
    pod/random-hash-wwp46   0/1     Completed   0          17s

Retrieve the generated hash from one of the Pods

    k logs pod/random-hash-wwp46 -n ckad-6
    ---
    MTgzMjIK