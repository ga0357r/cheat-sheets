# Kubernetes

## Kubernetes Code Cheat Sheet
### Deploy workload to cluster 
``` 
kubectl apply -f <deploymentName.yaml>
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
