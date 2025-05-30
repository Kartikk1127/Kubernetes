### Kubernetes Configuration YAML
1. Kubernetes configuration file (YAML or JSON)
2. Each file contains one or more manifests.
3. Each manifest describes an API object (deployment, job, secret).
4. Each manifest needs four parts (root key: values in the file)
   1. apiVersion:
   2. kind:
   3. metadata:
   4. spec:

### Building your YAML files
1. kind: We can get a list of resources the cluster supports.
   1. `kubectl api-resources`
2. apiVersion: We can get the API versions teh cluster supports.
   1. `kubectl api-versions`
3. metadata: only name is required.
4. spec: Where all the action is at!

### Building your YAML spec
1. We can get all the keys each **kind** supports
   1. `kubectl explain services --recursive`
2. A bit more detailed:
   1. `kubectl explain services.spec`
3. We can walk through the spec this way:
   1. `kubectl explain services.spec.type`
4. spec: can have sub spec: of other resources
   1. `kubectl explain deployment.spec.template.spec.volumes.nfs.server`
5. We can also use docs