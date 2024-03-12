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

### Push a customized Dockerfile to GKE
```
# create a Docker image with the requiredName
export PROJECT_ID=project-id
# export PROJECT_ID=learning-gke-413115

docker build -t gcr.io/${PROJECT_ID}/<requiredName>:<requiredVersion(v1)> .
# docker build -t africa-south1-docker.pkg.dev/learning-gke-413115/pob-server/testing/pob-server-web:v1 .

# Confirm that the build was successful.
docker images

# Test created image
docker run --rm -p 80:80 gcr.io/${PROJECT_ID}/<requiredName>:<requiredVersion : v1>
# docker run  africa-south1-docker.pkg.dev/learning-gke-413115/pob-server/testing/pob-server-db-backup:v1

# Push the container to GCR (Google Container Registry)
# enable the Container Registry API for your project.
gcloud services enable containerregistry.googleapis.com

# authenticate to GCR
gcloud auth configure-docker

# upload the Docker image.
docker push gcr.io/${PROJECT_ID}/gke-tutorial-image:v1
# docker push africa-south1-docker.pkg.dev/learning-gke-413115/pob-server/testing/pob-server-web:v1

# Check if the upload has been completed.
gcloud container images list
```

### Add permission to service account
```
# create a Docker image with the requiredName
gcloud projects add-iam-policy-binding <project_id> --member="serviceAccount:<fullServiceAccount>" --role="roles/<fullRole>"

#Example
gcloud projects add-iam-policy-binding learning-gke-413115 --member="serviceAccount:pull-images-from-registry@learning-gke-413115.iam.gserviceaccount.com" --role="roles/artifactregistry.reader"
```

### List images in other repositories
```
gcloud container images list --repository <registry_server_domain>/<project_id>/<repo_name>
# for example : gcloud container images list --repository africa-south1-docker.pkg.dev/learning-gke-413115/pob-server
```

### Create a Network File System(NFS) Server with Google Compute Engine persistent disks and mount them to your containers.
**1. Create Persistent Disk in Google Compute Engine**
```
# using gcloud cli
gcloud compute disks create --size=<sizeNumber> --zone=<zoneName> <diskName>(gce-nfs-disk)
# gcloud compute disks create --size=1GB --zone=africa-south1 pob-nfs-disk
```

**2. Create NFS Server in GKE**
```
# kubectl apply -f 001-nfs-server.yaml
# 001-nfs-server.yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: nfs-server
spec:
  replicas: 1
  selector:
    matchLabels:
      role: nfs-server
  template:
    metadata:
      labels:
        role: nfs-server
    spec:
      containers:
      - name: nfs-server
        image: gcr.io/google_containers/volume-nfs:0.8
        ports:
          - name: nfs
            containerPort: 2049
          - name: mountd
            containerPort: 20048
          - name: rpcbind
            containerPort: 111
        securityContext:
          privileged: true
        volumeMounts:
          - mountPath: /exports
            name: mypvc
      volumes:
        - name: mypvc
          gcePersistentDisk:
            pdName: gce-nfs-disk
            fsType: ext4
```


**3. Create NFS Service in GKE**
```
# kubectl apply -f 002-nfs-server-service.yaml
# 002-nfs-server-service.yaml
apiVersion: v1
kind: Service
metadata:
  name: nfs-server
spec:
  # clusterIP: 10.3.240.20
  ports:
    - name: nfs
      port: 2049
    - name: mountd
      port: 20048
    - name: rpcbind
      port: 111
  selector:
    role: nfs-server
```
Note down the cluster IP : "kubectl get svc nfs-server" # 10.23.241.98

**4. Create Persistent Volume and Persistent Volume Claims**
```
# kubect apply -f 003-pv-pvc.yaml
# 003-pv-pvc.yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: nfs
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteMany
  nfs:
    server: <ClusterIP> # Service discovery name nfs-service.default.svc.cluster.local.
    path: "/"

---
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: nfs
spec:
  accessModes:
    - ReadWriteMany
  storageClassName: ""
  resources:
    requests:
      storage: 1Gi
```

**5. Configuring Pods with PVC**
```
# kubect apply -f nfs-busybox.yaml
# nfs-busybox
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: nfs-busybox
spec:
  replicas: 1
  selector:
    matchLabels:
      name: nfs-busybox
  template:
    metadata:
      labels:
        name: nfs-busybox
    spec:
      containers:
      - image: busybox
        imagePullPolicy: Always
        name: busybox
        volumeMounts:
          # name must match the volume name below
          - name: my-pvc-nfs
            mountPath: "/mnt"
      volumes:
      - name: my-pvc-nfs
        persistentVolumeClaim:
          claimName: nfs
```

