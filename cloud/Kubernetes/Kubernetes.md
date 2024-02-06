# Kubernetes

## Kubernetes Code Cheat Sheet
### Deploy workload to cluster 
``` 
kubectl apply -f <deploymentName.yaml>
```

### Stop running a workload in cluster 
``` 
kubectl scale --replicas=0 deployment/<deploymentname.yaml>
```
