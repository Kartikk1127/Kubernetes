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

## Imperative vs Declarative
1. **Imperative**: Focus on how a program operates.
2. **Declarative**: Focus on what a program should accomplish.
3. Example: "I'd like a cup of coffee"
4. **Imperative**: I boil water, scoop out 42 grams of medium-fine grounds, poor over 700 gms of water, etc.
5. **Declarative**: "Barista, I'd like a cup of coffee". (Barista is the engine that works through the steps, including retrying to make a cup, and is only finished when I have a cup)

### Kubernetes Imperative
1. Examples: `kubectl run`,`kubectl create deployment`, `kubectl update`.
   1. We start with a state we know(no deployment exists).
   2. We ask kubectl run to create a deployment.
2. Different commands are required to change that deployment.
3. Different commands are required per object.
4. Imperative is easier when you know the state.
5. Imperative is easier to get started.
6. Imperative is easier for humans at the CLI.
7. Imperative is NOT easy to automate.

### Kubernetes Declarative
1. Example: `kubectl apply -f my-resources.yaml`
   1. We don't know the current state.
   2. We only know what we want the end result to be (yaml contents).
2. Same command each time (tiny exception for delete).
3. Resources can be all in a file, or many files (apply a whole directory).
4. Requires understanding the YAML keys and values.
5. More work than `kubectl run` for just starting a pod.
6. The easiest way to automate.
7. The eventual path to GitOps happiness.

## Three Management Approaches
1. **Imperative commands**: `run, expose, scale, edit, create deployment`
   1. Best for dev/learning/personal projects.
   2. Easy to learn, hardest to manage over time.
2. **Imperative objects**: `create -f file.yml, replace -f file.yml, delete`...
   1. Good for prod of small environments, single file per command.
   2. Store your changes in git-based yaml files.
   3. Hard to automate.
3. **Declarative objects**: `apply -f file.yml` or `dir\, diff`
   1. Best for prod, easier to automate.
   2. Harder to understand and predict changes.

### Advice
1. Don't mix the above 3 approaches.
2. Learn the imperative CLI for easy control of local and test setups.
3. Move to **apply -f file.yml** and **apply -f directory** for prod.
4. Store yaml in git, git commit each change before you apply.
5. This trains you for later doing GitOps (where git commits are automatically applied to clusters)