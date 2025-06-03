## Kubernetes in 1 Shot

1. The containers run on the worker node. None of them work on the master node.

### Master Node has:
1. **Api Server :** (acts as a gateway between the components on master node and the kubernetes cluster).
2. **Scheduler :** (schedules the containers/pods on the worker nodes).
3. **ETCD :** (A key value pair data store that stores the data of the k8s cluster).
4. **Controller Manager :** (Is responsible for ensuring everything inside a cluster be it a pod, node, services, data store are working fine or not).

### Worker Node has:
1. **Kubelet :** Works at a _worker node level_. Ensures whether the pod is running fine or not and communicates the same with the API Gateway which eventually stores the data on etcd.
2. **Service Proxy :** In order to access the Pods as a user, you need a service proxy.

**Note:** All the nodes(master node-worker node) communicate with each other via CNI(Container Network Interface) network. Eg: _Weave Net, Calico_.

### Ways to create Kubernetes Cluster
1. Kubeadm
2. Minikube (local/ec2)
3. KIND cluster
4. EKS(Elastic Kubernetes Service)/AKS(Azure Kubernetes Service)/GKE(Google Kubernetes Enterprise)

### ReplicaSet vs StatefulSet vs Deployment vs DaemonSet
1. ReplicaSet: Ensures a specified number of identical pods are always running. It’s mainly used by Deployments to manage pod scaling and replication.
2. Deployment: Manages ReplicaSets and allows easy updates(rolling updates) and rollbacks of applications. It’s the most common way to manage stateless applications.
3. StatefulSet: Like Deployment, but used for stateful applications. It gives each pod a stable identity, persistent storage, and ordered startup/shutdown.
4. DaemonSet: Makes sure that one replica is always running on a worker.

In short:
1. ReplicaSet = Keeps pods running.
2. Deployment = Manages stateless apps with ReplicaSets.
3. StatefulSet = Manages stateful apps with identity and storage.