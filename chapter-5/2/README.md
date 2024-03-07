# Exercise 2
Create a YAML manifest for a Pod named loop that runs the busybox:1.36.1 image in a container. The container should run the following command: for i in {1..10}; do echo "Welcome $i times"; done. 

     kubectl run loop --image=busybox:1.36.1 -o yaml --dry-run=client \
     --restart=Never -- /bin/sh -c 'for i in 1 2 3 4 5 6 7 8 9 10; \
     do echo "Welcome $i times"; done' \
     > loop3.yaml

Create the Pod from the YAML manifest. What’s the status of the Pod?

     k apply -f loop3.yaml  -n ckad
     k get pod -n ckad

     NAME    READY   STATUS      RESTARTS      AGE
     loop    0/1     Completed   0             22s
     nginx   1/1     Running     1 (15m ago)   22h
     rr      0/1     Completed   0             22h

Edit the Pod named loop. Change the command to run in an endless loop. Each iteration should echo the current date.

    k apply  -f loop4.yaml  -n ckad
    k get pod -n ckad

    NAME    READY   STATUS      RESTARTS      AGE
    loop    0/1     Completed   0             11m
    loop4   1/1     Running     0             6s
    nginx   1/1     Running     1 (26m ago)   22h
    rr      0/1     Completed   0             22h

Inspect the events and the status of the Pod loop.

    k describe pod loop4   -n ckad

    Events:
    Type    Reason     Age    From               Message
    ----    ------     ----   ----               -------
    Normal  Scheduled  2m50s  default-scheduler  Successfully assigned ckad/loop4 to minikube
    Normal  Pulled     2m50s  kubelet            Container image "busybox:1.36.1" already present on machine
    Normal  Created    2m50s  kubelet            Created container loop4
    Normal  Started    2m50s  kubelet            Started container loop4

    k logs  loop4   -n ckad

    Thu Mar  7 02:02:07 UTC 2024
    Thu Mar  7 02:02:12 UTC 2024
    Thu Mar  7 02:02:17 UTC 2024
    Thu Mar  7 02:02:22 UTC 2024
    Thu Mar  7 02:02:27 UTC 2024
    Thu Mar  7 02:02:32 UTC 2024
