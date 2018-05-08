# Lab 1. Set up and deploy your first application

Learn how to push an image of an application to IBM Cloud Container Registry and deploy a basic application to a cluster.

# Install Prerequisite CLIs and Provision a Kubernetes Cluster

If you haven't already:
1. Install the CLIs and Docker, as described in [Lab 0](../Lab0/README.md).
2. Provision a cluster: 

   ```bx cs cluster-create --name <name-of-cluster>```


# Deploy your application

1. Run `bx cs cluster-config <yourclustername>`, and set the variables based on the output of the command.

2. Start by running your image as a deployment: 

   ```kubectl run guestbook --image=ibmcom/guestbook:v1```
   
   This action will take a bit of time. To check the status of your deployment, you can use `kubectl get pods`.

   You should see output similar to the following:
   
   ```
   => kubectl get pods
   NAME                          READY     STATUS              RESTARTS   AGE
   guestbook-59bd679fdc-bxdg7    0/1       ContainerCreating   0          1m
   ```

3. Once the status reads `Running`, expose that deployment as a service, accessed through the IP of the worker nodes.  The example for this course listens on port 8080.  Run:

   ```kubectl expose deployment/guestbook --type="NodePort" --port=3000```

4. To find the port used on that worker node, examine your new service: 

   ```kubectl describe service <name-of-deployment>```

   Take note of the "NodePort:" line as `<nodeport>`

5. Run `bx cs workers <name-of-cluster>`, and note the public IP as `<public-IP>`.

6. You can now access your container/service using `curl <public-IP>:<nodeport>` (or your favorite web browser). You should see the html output from the guestbook application. 

When you're all done, you can either use this deployment in the [next lab of this course](../Lab2/README.md), or you can remove the deployment and thus stop taking the course.

1. To remove the deployment, use `kubectl delete deployment guestbook`. 
2. To remove the service, use `kubectl delete service guestbook`.
