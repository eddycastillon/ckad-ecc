# Volumes

Create a Pod YAML file with two containers that use the image alpine:3.12.0. Provide a command for both containers that keep them running forever.

    kubectl run multi-container-pod --image=alpine:3.12.0 -o yaml --dry-run=client -- /bin/sh -c 'while true; do echo "hi wolrd";sleep 60;done'  > multi-container-pod.yaml

Edit the yaml file and add a second container. Duplicate the lines create for the default container.

    kubectl  apply -f multi-container-pod.yaml  -n ckad-7
    
---

Define a Volume of type emptyDir for the Pod. Container 1 should mount the Volume to path /etc/a, and container 2 should mount the Volume to path /etc/b.

    kubectl  delete -f multi-container-pod.yaml  -n ckad-7

    kubectl  apply -f multi-container-pod-1.yaml  -n ckad-7

---

Open an interactive shell for container 1 and create the directory data in the mount path. Navigate to the directory and create the file hello.txt with the contents “Hello World.” Exit out of the container.

Container 1

    kubectl exec -it  multi-container-pod  -n ckad-7 -c multi-container
    -pod-1  -- /bin/sh

    mkdir -p /etc/a/data
    cd /etc/a/data
    echo "Hello World" > hello.txt

---

Open an interactive shell for container 2 and navigate to the directory /etc/b/data. Inspect the contents of file hello.txt. Exit out of the container.

Container 2

    kubectl exec -it  multi-container-pod  -n ckad-7 -c multi-container
    -pod-1  -- /bin/sh

    cat /etc/b/data/hello.txt