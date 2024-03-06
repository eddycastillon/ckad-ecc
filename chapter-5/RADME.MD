# Exercise 1
## Pod and namespace

Create a new Pod named nginx running the image nginx:1.17.10. Expose the container port 80. The Pod should live in the namespace named ckad.

    k create namespace ckad --dry-run=client -o yaml > ns-ckad.yaml
    
    k run  nginx --image=nginx:1.17.10 --dry-run=client --port=80 -o yaml > pod.yaml
    k apply  -f  ns-ckad.yaml
    k apply  -f pod.yaml -n ckad
     
    k get pod -n ckad

Get the details of the Pod including its IP address.

    k get pod -n ckad -o wide

Create a temporary Pod that uses the busybox:1.36.1 image to execute a wget command inside of the container. The wget command should access the endpoint exposed by the nginx container. You should see the HTML response body rendered in the terminal.

    kubectl debug -it nginx --image=busybox:1.36.1 --target=nginx -n ckad

        Targeting container "nginx". If you don't see processes from this container it may be because the container runtime doesn't support this feature.
        Defaulting debug container name to debugger-bnmr7.
        If you don't see a command prompt, try pressing enter.
        / #
        / #
        / # wget localhost
        Connecting to localhost (127.0.0.1:80)
        saving to 'index.html'
        index.html           100% |************************************************************************************************|   612  0:00:00 ETA
        'index.html' saved

    k run  debug-nginx --image=busybox:1.36.1  --rm -it --restart=Never  -n ckad

Get the logs of the nginx container.

    k logs nginx -n ckad
        Defaulted container "nginx" out of: nginx, debugger-24bb5 (ephem), debugger-bnmr7 (ephem)
        127.0.0.1 - - [06/Mar/2024:03:05:15 +0000] "GET / HTTP/1.1" 200 612 "-" "Wget" "-"

Add the environment variables DB_URL=postgresql://mydb:5432 and DB_USERNAME=admin to the container of the nginx Pod.

    k delete -f pod.yaml  -n ckad
    k apply  -f pod-v1.yaml  -n ckad
    k exec -it nginx -n ckad -- env | grep  -i DB
    
    ** Output **
    DB_URL=postgresql://mydb:5432
    DB_USERNAME=admin

Open a shell for the nginx container and inspect the contents of the current directory ls -l. Exit out of the container.

    k exec -it nginx -n ckad -- sh
    ls -l



