# Kubernetes

## What is Kubernetes?
1. Popular container orchestrator.
2. Container orchestration = Make many servers act like one.
3. Released by Google in 2015, maintained by large community.
4. Runs on top of Docker (usually) as a set of APIs in containers.
5. Provides API/CLI to manage containers across servers.
6. Many clouds provide it for you.
7. Many vendors make a "distribution" of it.

## Why Kubernetes?
1. Why you may need orchestration?
2. Not every solution needs orchestration.
3. Servers+Change Rate = Benefit of orchestration.
4. Then, decide which orchestrator.
5. If kubernetes, decide which distribution
   1. cloud or self-managed(Docker Enterprise, Rancher, OpenShift, Canonical, VMWare PKS)
   2. Don't usually need pure upstream.

## Kubernetes or Swarm?
1. Kubernetes and Swarm are both container orchestrators.
2. Both are solid platforms with vendor backing.
3. Swarm: Easier to deploy/manage.
4. Kubernetes: More features and flexibility.
5. What's right for you? Understand both and know your requirements.

### Advantages of Swarm
1. Comes with Docker, single vendor container platform.
2. Easiest orchestrator to deploy/manage yourself.
3. Follows 80/20 rule, 20% of features for 80% of use cases.
4. Runs anywhere Docker does:
   1. local, cloud, datacenter
   2. ARM, Windows, 32-bit 
5. Secure by default.
6. Easier to troubleshoot.

### Advantages of Kubernetes
1. Clouds will deploy/manage Kubernetes for you.
2. Infrastructure vendors are making their own distributions.
3. Widest adoption and community.
4. Flexible: Covers widest set of use cases.
5. "Kubernetes first" vendor support.
6. "No one ever got fired for buying IBM".
   1. Picking solutions isn't 100% rational.
   2. Trendy, will benefit your career.
   3. CIO/CTP Checkbox.

### Terminology
1. **Kubernetes**: The whole orchestration system.
   1. K8s "k-eights" or _Kube_ for short.
2. **Kubectl**: CLI to configure Kubernetes and manage apps.
   1. Using "cube control" official pronunciation.
3. **Node**: Single server in the Kubernetes cluster.
4. **Kubelet**: Kubernetes agent running on nodes.
5. **Control Plane**: Set of containers that manage the cluster.
   1. Includes API server, scheduler, controller manager, etcd, and more.
   2. Sometimes called the "master".

### Installation
1. Kubernetes is a series of containers, CLI's and configurations.
2. Many ways to install.
3. Docker Desktop: Enable in settings
   1. Sets up everything inside Docker's existing Linux VM.  

### Kubernetes Container Abstractions
1. **Pod :** One or more containers running together on one Node.
   1. Basic unit of deployment. Containers are always in pods.
2. **Controller :** For creating/updating pods and other objects.
   1. Many types of controllers inc. Deployment, ReplicaSet, StatefulSet, DaemonSet, Job, CronJob, etc.
3. **Service :** Network endpoint to connect to a pod.
4. **Namespace :** Filtered group of objects in cluster.
5. Secrets, ConfigMaps, and more.

## Your First Pods

### Kubernetes Run, Create, and Apply
1. Kubernetes is evolving, and so is the CLI.
2. We get three ways to create pods from the _kubectl_ CLI.
   1. `kubectl run` (single pod per command since 1.18)
   2. `kubectl create` (create some resources via CLI or YAML)
   3. `kubectl apply` (create/update anything via YAML)
3. For now, we'll just use _run_ or _create_ CLI.

## Resource Generators
1. These commands use helper templates called "generators".
2. Every resource in Kubernetes has a specification or "spec".
3. Eg: `kubectl create deployment sample --image nginx --dry-run=client -o yaml`
4. You can output those templates with `--dry-run=client -o yaml`
5. You can use those YAML defaults as a starting point.
6. Generators are "opinionated defaults"

### Generator Examples
1. Using dry-run with yaml output we can see the generators.
   1. `kubectl create deployment test --image nginx --dry-run=client -o yaml`
   2. `kubectl create job test --image nginx --dry-run=client -o yaml`
   3. `kubectl expose deployment/test --port 80 --dry-run=client -o yaml` [You need the deployment to exist before this works]