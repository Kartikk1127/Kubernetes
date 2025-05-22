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
7. 