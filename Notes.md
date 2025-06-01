# Important Commands
1. `kubectl version`
2. `kubectl run my-nginx --image nginx` : Will create a pod for nginx web-server.
3. `kubectl get pods` : Will list you the pods.
4. `kubectl get all` : Will list you the resources.
5. `kubectl create deployment my-nginx --image nginx` : Will create a deployment > replicaSet > Pod.
6. `kubectl delete <pod/deployment>` : Will delete the pod or the deployment you want to delete.
7. `kubectl run pingpong --image alpine --command -- ping 1.1.1.1` : Create a single pod with a custom command.
8. `kubectl create deployment pingpong --image alpine -- ping 1.1.1.1` : Create a deployment with a custom command.
9. `kubectl get <pod/deployment>` : Will list the specific resources of the pod or the deployment you wanted.
10. `kubectl get all -o wide` : Show more with a "wide" output.
11. `kubectl describe deploy/my-apache` : 
    1. Deployment summary
    2. ReplicaSet status
    3. Pod template
    4. Old/new replicaSet names.
    5. Deployment Events.
12. `kubectl get pods -w` : To watch changes in real time
13. `kubectl get events --watch-only` : Better alternative to the above command. 
14. `kubectl logs deploy/my-apache` : To view the kubernetes container logs.
15. `kubectl logs deploy/my-apache --follow --tail 1` : To view logs in real time with the last one line.
16. `kubectl logs pod/my-apache-xxx-yyy -c httpd` : Get a specific container's logs in a pod.
17. `kubectl logs pod/my-apache --all-containers=true` : Get logs of all containers in a pod.
18. `kubectl logs -l app=my-apache` : Get multiple pod logs.

# Notes
1. Unlike docker, in order to run a pod you always have to give it a name. There are no random names.
2. The idea of pods is more or less the abstraction layer that runs one or more containers within it that share the same IP address and same deployment mechanism.
3. Unlike Docker, you can't create a container directly in K8s.
4. You create Pod(s) (via CLI, YAML, or API)
   1. Kubernetes then creates the container(s) inside it.
5. `kubelet` is the agent that tells the container runtime to create containers for you.
6. Every type of resource to run container uses Pods.
7. Mostly use `kubectl get <pod/deployment/service> -o yaml/wide` to inspect and understand the resources well.
8. Selectors and Labels connect a deployment to its replica sets, and a replica set to its pods.
9. `kubectl run` command only creates a single pod, with a single container from a single image.
10. NodePort ports range from 30000-32767

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

### Inspecting resources with Describe
1. `kubectl get` has a weakness
   1. It can only show one resource at a time.
2. We need a command that combines related resources.
   1. Parent/Child resources.
   2. Events of that resources.
3. A typical workflow of "high level" to "low level" details.
4. List all our clusters nodes
   1. `kubectl get nodes`
5. List just our node, with more details
   1. `kubectl get node/desktop-control-plane -o wide`
6. Describe the node
   1. `kubectl describe node/desktop-control-plane`

### Exposing Containers
1. `kubectl expose` creates a service for existing pods.
2. A service is a stable address for pod(s).
3. If we want to connect to pod(s), we need a service.
4. CoreDNS allows us to resolve services by name.
5. There are different types of services:
   1. ClusterIP
   2. NodePort
   3. LoadBalancer
   4. ExternalName

### Basic Service Types
1. **ClusterIP**(default)
   1. Single, internal virtual IP allocated.
   2. Only reachable from within cluster(nodes and pods).
   3. Pods can reach service on apps port number.
2. **NodePort**
   1. High port allocated on each node.
   2. Port is open on every node's IP.
   3. Anyone can connect (if they can reach node).
   4. Other pods need to be updated to this port.
3. These services are always available in Kubernetes.
4. **Load Balancer**
   1. Controls a LB endpoint external to the cluster.
   2. Only available when infra provider gives you a LB(AWS ELB etc).
   3. Creates NodePort+ClusterIP services, tells LB to send to NodePort.
5. **ExternalName**
   1. Adds CNAME DNS record to CoreDNS only.
   2. Not used for Pods, but for giving pods a DNS name to use for something outside Kubernetes.
6. **Kubernetes Ingress**

### Creating a ClusterIP Service
1. Open two shell windows so we can watch this
   1. `kubectl get pods -w`
2. In second window, lets start a simple http server using sample code
   1. `kubectl create deployment httpenv --image=bretfisher/httpenv`
3. Scale it to 5 replicas
   1. `kubectl scale deployment/httpenv --replicas=5`
4. Let's create a ClusterIP service (default)
   1. `kubectl expose deployment/httpenv --port 8888`
5. `kubectl get service`
6. Now, we will curl the ClusterIP service
   1. `kubectl run tmp-shell --rm -it --image bretfisher/netshoot -- bash`
   2. `curl httpenv:8888`

### Create a NodePort Service
1. Let's expose a NodePort, so we can access it via the host IP (including localhost on Windows/Linux/macOS).
   1. `kubectl expose deployment/httpenv --port 8888 --name httpenv-np --type NodePort` 
2. Did you know that a NodePort service also creates a ClusterIP?
3. These three service types are additive, each one creates the one above it:
   1. ClusterIP
   2. NodePort
   3. LoadBalancer
4. Note:
   1. Even after exposing the nodeport service, if you are using docker desktop on a windows machine you might not be able to curl on <node-ip>:<node-port>
   2. This is because the docker desktop runs behind a VM and not directly on the host.
   3. So even though node-port is exposed, it is not exposed on your host but on your VM.
   4. In order to curl, you need to do port forwarding.
   5. Execute: `kubectl port-forward services/httpenv 8888:8888`
   6. Now, in another terminal you can go on to curl `curl localhost:8888`

### Kubernetes Service DNS
1. Internal DNS is provided by CoreDNS.
2. Like Swarm, this is DNS-Based Service Discovery.
3. So far we've been using hostnames to access Services: `curl <hostname>`
4. But that only works for Services in the same namespace: `kubectl get namespaces`
5. Services also have a Fully Qualified Domain Name(FQDN): `curl <hostname>.<namespace>.svc.cluster.local`
 
## Moving to Declarative Kubernetes YAML

### kubectl apply
1. `kubectl apply -f myfile.yaml`[create/update resources in a file]
2. `kubectl apply -f myyaml/`[create/update a whole directory of yaml]
3. `kubectl apply -f https.//bret.run/pod.yml`[create/update from a URL]
4. Be careful before applying changes to a pod via url. Try curling it first.

## Storage in Kubernetes
1. Storage and stateful workloads are harder in all systems.
2. Containers make it both harder and easier than before.
3. **StatefulSets** is a new resource type, making Pods more sticky.
4. Recommendation: Avoid stateful workloads for first few deployments until you're good at the basics.
5. Use db-as-a-service whenever you can.

### Volumes in Kubernetes
1. Creating and connecting Volumes: 2 types.
2. **Volumes**
   1. Tied to lifecycle of a Pod.
   2. All containers in a single Pod can share them.
3. **PersistentVolumes**
   1. Created at the cluster level, outlives a Pod.
   2. Separates storage config from Pod using it.
   3. Multiple pods can share them.
4. CSI plugins are the new way to connect to storage.

## Higher Deployment Abstractions
1. All our kubectl commands just talk to the kubernetes API.
2. Kubernetes has limited built-in templating, versioning, tracking, and management of your apps.
3. There are now over 60 3rd party tools to do that, but many are defunct.
4. **Helm** is the most popular.
5. **Compose on Kubernetes** comes with Docker desktop.
6. Remember these are optional, and your distro may have a preference.
7. Most distros support **Helm**






