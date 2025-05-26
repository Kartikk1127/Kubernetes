# Important Commands
1. `kubectl version`
2. `kubectl run my-nginx --image nginx` : Will create a pod for nginx web-server.
3. `kubectl get pods` : Will list you the pods.
4. `kubectl get all` : Will list you the resources.
5. `kubectl create deployment my-nginx --image nginx` : Will create a deployment > replicaSet > Pod.
6. `kubectl delete <pod/deployment>` : Will delete the pod or the deployment you want to delete.

# Notes
1. Unlike docker, in order to run a pod you always have to give it a name. There are no random names.
2. The idea of pods is more or less the abstraction layer that runs one or more containers within it that share the same IP address and same deployment mechanism.
3. Unlike Docker, you can't create a container directly in K8s.
4. You create Pod(s) (via CLI, YAML, or API)
   1. Kubernetes then creates the container(s) inside it.
5. `kubelet` is the agent that tells the container runtime to create containers for you.
6. Every type of resource to run container uses Pods.

## Scaling ReplicaSets
1. Start a new deployment for one replica/pod
   1. `kubectl create deployment my-apache --image httpd`
2. Check our resource status
   1. `kubectl get all`
3. Let's scale it up with another pod
   1. `kubectl scale deploy/my-apache --replicas 2` OR
   2. `kubectl scale deploy my-apache --replicas 2`
4. The above 2 are same commands.
5. deploy=deployment=deployments.

### What happened when we scaled?
1. kubectl scale will "change" the deployment/my-apache record.
2. CM(Configuration Manager) will see that "only" replica count has changed.
3. It will change the number of pods in ReplicaSet.
4. Scheduler sees a new pod is requested, assigns a node.
5. Kubelet sees a new pod, tells container runtime to start httpd.