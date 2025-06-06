1. A **pod** can run one or more **containers**.
2. Since k8 auto-scales and auto heales, hence you need a **deployment**
3. Then you also need a **service**.
4. Here's the step-by-step procedure:
   1. For a container(s) you need a pod.
   2. For a pod, you need a deployment.
   3. For a deployment, you need a service.
5. A **namespace** in Kubernetes is a unit that holds all the resources for that particular group(let's say for nginx--pod, deployment, service). This helps in managing the resources.
6. **Ingress** helps in re-routing traffic.
7. 