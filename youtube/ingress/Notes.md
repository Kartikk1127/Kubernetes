1. In order to set up routing of various endpoints to different services like a microservices architecture, we need ingress.
2. To setup ingress, first make sure the services are running on the same namespaces.
3. Then we need to make a ingress controller.

### Commands
1. `kubectl apply -f https://kind.sigs.k8s.io/examples/ingress/deploy-ingress-nginx.yaml`
2. `kubectl get pods -n ingress-nginx`  --  the namespace will automatically be created upon the execution of the above command.
3. `kubectl get services -n ingress-nginx`  -- services will automatically be created.
4. Now create the `ingress.yml`
5. Now, port forward the controller: `sudo -E kubectl port-forward service/ingress-nginx-controller -n ingress-nginx 80:80 --address=0.0.0.0`
6. If you have the setup on your ec2, go on to edit the inbound rules and allow the exact port you forward in the above command.
