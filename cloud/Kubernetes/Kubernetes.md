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
# copy key to minikube file system
docker cp .\pull-images-from-registry-key.json minikube:/home/docker/pull-images-from-registry-key.json

kubectl create secret docker-registry pob-server-repo-key --docker-server=https://africa-south1-docker.pkg.dev --docker-username=_json_key --docker-password "$(docker exec -it minikube cat /home/docker/pull-images-from-registry-key.json)" --docker-email=nonexistent@example.com

# apply the secret to namespace
apiVersion: v1
kind: ServiceAccount
metadata:
  name: default
imagePullSecrets:
- name: pob-server-repo-key


# delete secret
kubectl delete secret pob-server-repo-key
```

### Port Forward 
```
kubectl port-forward service/<activeServiceName> 80:80
```

### Display endpoint url link for service 
```
minikube service <activeServiceName> --url
```

### Enable network route
```
minikube tunnel
```


