# Google Kubernetes Engine(GKE)
(GKE) is a service that allows you to use managed Kubernetes clusters for deploying and managing the operation of containerized applications on Google Cloud's infrastructure.

A GKE environment consists of VMs grouped together to form a cluster. Apps are packaged into containers and the kubernetes API is used to interact with the app.

It is recommended to run GKE in autopilot mode so the control plane and worker nodes are automatically managed

## GKE Cluster Architecture
When a GKE cluster is created, two types of components are automatically built: control plane (master node) and nodes (worker nodes).

### control plane (master node)
The control plane for a GKE cluster is created within a managed VPC in Google Cloud, and there is no burden on users to build or operate it.

### nodes (worker nodes)
A cluster contains one or more nodes that run workloads.
In a GKE cluster, nodes are configured by Compute Engine instances and are automatically created during cluster creation.
Auto-healing is enabled by default on nodes and performs periodic health checks on each node in the cluster.

https://blog.g-gen.co.jp/entry/gke-ingress-using-google-managed-ssl-cert#GKE-クラスタの準備

https://blog.g-gen.co.jp/entry/gke-explained#Google-Kubernetes-Engine-とは

https://cloud.google.com/kubernetes-engine/docs/how-to/managed-certs

## Ingress
Ingress is a Kubernetes API resource that provides L7 load balancing for workloads within a Kubernetes cluster.

## Use cases for GKE
- AI and ML operations
- Data processing at scale
- Scalable online games platforms
- Reliable applications under heavy load

## Get Started with GKE
### Before you begin
1) Create a Google cloud account
2) Create a Google cloud project by going to "Manage Resources > Create New Project"
3) Make sure billing is enabled for google cloud project.
4) Enabled artifact registry and kubernetes and kubernetes engine api

### Launch Cloud Shell
1) Go to Google Cloud Console
2) Click activate Cloud Shell Button on the upper right corner of the console.
3) Set default project in Google cloud CLI with ```gcloud config set project PROJECT_ID```

### Create a GKE cluster
A cluster consist of a control node and a worker node. You deploy applications to clusters, and the applications run on the nodes.

1) Create an Autopilot cluster named hello-cluster  ```gcloud container clusters create-auto hello-cluster \
    --location=us-central1```
2) Get authentication credentials to interact with the cluster ``` gcloud container clusters get-credentials hello-cluster \
    --location us-central1 ```

### Deploy an application to the cluster
1) To run your container in the cluster, you need to deploy the container by running the following command ``` kubectl create deployment hello-server \
    --image=us-docker.pkg.dev/google-samples/containers/gke/hello-app:1.0 ```

2) Expose the Deployment - After deploying the application, you need to expose it to the internet so that users can access it.
You can expose your application by creating a Service, a Kubernetes resource that exposes your application to external traffic.

```
kubectl expose deployment hello-server \
--type LoadBalancer \
--port 80 \
--target-port 8080
```

N.B : A load balancer distributes user traffic across multiple instances of your applications

### Inspect and view the application
1) Inspect the running Pods ``` kubectl get pods ```
2) Inspect the hello-server Service ``` kubectl get service hello-server ```
3) View the application from your web browser by using the external IP address with the exposed port ``` http://EXTERNAL_IP ```

### Clean up
To avoid incurring charges to your Google Cloud account for the resources used which are no longer being used
1) Delete the application's Service ``` kubectl delete service hello-server ```
2) Delete your cluster ``` gcloud container clusters delete hello-cluster \
    --location us-central1 ```

## GCP Code Cheat Sheet
### Delete your cluster 
``` 
gcloud container clusters delete <clusterName> \
<clusterLocation>
```

### Assign a static IP address 
```
# Assign the static IP address
gcloud compute addresses create <addressName> --global

# Check the assigned IP address
gcloud compute addresses describe <addressName> --global
```

### Get all managed zones 
```
gcloud dns managed-zones list
```

### Register a record
```
# Start transaction
gcloud dns record-sets transaction start --zone=<zoneName>

# Register a record
gcloud dns record-sets transaction add <staticIpAdress> \
   --name=<domainName e.g example.com> \
   --ttl=300 \
   --type=A \
   --zone=<zoneName>

# End transaction
gcloud dns record-sets transaction execute --zone=<zoneName>
```

### Abort a transaction
```
# Start transaction
gcloud dns record-sets transaction start --zone=<zoneName>

# Abort transaction
gcloud dns record-sets transaction abort --zone=<zoneName>
```
