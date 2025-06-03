
1. `sudo usermod -aG <group name> $USER && newgrp docker` [Helps in giving access to the user]
2. `kubectl get nodes` [Can use to get the nodes]
3. `kubectl get namespaces` OR `kubectl get ns`
4. `kubectl create ns nginx`
5. `kubectl run nginx --image=nginx` [Creates an nginx pod]
6. `kubectl get pods`
7. `kubectl delete pod <image name>` [Any image present in docker hub]
8. `kubectl apply -f <yaml file>` [Helps in creating namespaces which you can push on github]
9. `kubectl exec -it <pod name> -n <namespace> -- sh` [To ssh into the pod]
10. `kubectl describe pod <pod name> -n <namespace>` [In order to inspect your pod]
11. `kubectl scale deployment <deployment name> -n <namespace> --replicas=<desired count>` [To scale the number of pods]
12. `kubectl get pods -n <namespace> -o wide` [Will fetch you more information about the pod]
13. `kubectl set image deployment/<deployment name> -n <namespace> <container name>=<new image with tag>` [Deployment makes it possible to update the images at runtime without causing any downtime]
14. 