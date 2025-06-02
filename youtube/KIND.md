## Steps to run kubernetes via KIND(Kubernetes in Docker)

1. Create an ec2 instance(t2 medium minimum since it is a bit heavy)
2. SSH into the instance.
3. Install KIND and Kubectl
   1. [Click here](https://github.com/LondheShubham153/kubestarter/tree/main/kind-cluster)
4. Update the system
   1. `sudo apt-get update`
5. Install docker
   1. `sudo apt-get install docker.io`
   2. `sudo usermod -aG docker $USER && newgrp docker` (To give permissions so as to run docker commands without sudo)
6. That's it. You have installed all the required things.
7. Next, create a directory
   1. `mkdir kind-cluster`
   2. `cd kind-cluster`
8. Create a config yaml file inside this directory
   1. `vim config.yml`
   2. Add the following configuration Create a file named ``kind-config.yaml`` with the following content:
    ```
    kind: Cluster 
    apiVersion: kind.x-k8s.io/v1alpha4 
    nodes: 
    - role: control-plane 
      image: kindest/node:v1.31.2 
    - role: worker 
      image: kindest/node:v1.31.2 
    - role: worker 
      image: kindest/node:v1.31.2 
    - role: worker 
      image: kindest/node:v1.31.2 
      extraPortMappings: 
      - containerPort: 80 
        hostPort: 80 
        protocol: TCP 
      - containerPort: 443 
        hostPort: 443 
        protocol: TCP 
    ```
9. Create a cluster now
   1. `kind create cluster --name=<cluster name>--config=config.yml`