# Kubernetes Best Practices
## Setting Resources and limits in Kubernetes
ResourceQuotas
```
spec:
  hard:
    requests.cpu: 500m
    requests.memory: 100Mib
    limits.cpu: 700m
    limits.memory: 500Mib
 ```
 
## Upgrading Kubernetes clusters with zero downtime: GCP
* Step 1 - Master
```
```
## Name Spaces
```

```
## Termination Lifecycle
https://www.youtube.com/watch?v=Z_l_kE1MDTc
default terminationGracePeriodSeconds: 30 
increase this value if the pods takes longer to terminate
```
terminationGracePeriodSeconds: 60
```
