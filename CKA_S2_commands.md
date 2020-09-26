# Create an NGINX Pod
``` kubectl run nginx --image=ngnix```
# Don't create 
```kubectl run nginx --image=nginx --dry-run=client -o yaml```
# Create a deployment
```kubectl create deployment --image=nginx nginx
```
# dry run
```kubectl create deployment --image=nginx nginx --dry-run=client -o yaml > outfile.yaml
```

# Create a Service named redis-service of type ClusterIP to expose pod redis on port 6379
*This will automatically use the pod's labels as selectors*
```kubectl expose pod redis --port=6379 --name redis=service --dry-run=client -o yaml```

```kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml```


