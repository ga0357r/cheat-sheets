# Kubernetes

## Kubernetes Code Cheat Sheet
### Deploy workload to cluster 
```
# if current directory is where the deployment is 
kubectl apply -f <deploymentName.yaml>

# if deployment is in a subfolder in the cd
kubectl apply -f .\<subFolder>\<deploymentName.yaml>
```

### Stop running a workload in cluster 
```
// deployments
kubectl scale --replicas=0 deployment/<deploymentName.yaml>
kubectl delete deployment <deploymentName>

// services
kubectl delete service <serviceName>

//ingress
kubectl delete ingress <ingressName>
```

### Get Workloads in a cluster 
```
//deployments
kubectl get deployments <deploymentName>

// services
kubectl get services <serviceName>

//ingress
kubectl get ingresses <ingressName>
```

### load a built image onto minikube 
```
minikube image load builtImagaName

```

### Create a secret 
```
$jsonKey = Get-Content .\pull-images-from-registry-key.json | ConvertFrom-Json
$password = $jsonKey.private_key

kubectl create secret docker-registry pob-server-key --docker-email=any@valid.email --docker-username=_json_key --docker-password=$password --docker-server=africa-south1-docker.pkg.dev

# apply the secret to namespace
apiVersion: v1
kind: ServiceAccount
metadata:
  name: default
imagePullSecrets:
- name: pob-server-key
```
