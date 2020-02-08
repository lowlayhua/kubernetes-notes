# 3. Scheduling
## Create a taint on node01 with key of 'spray', value of 'mortein' and effect of ‘NoSchedule'
```
kubectl taint nodes node01 spray=mortein:NoSchedule
node/node01 tainted
```
## Node Affinity
Which nodes are the PODs placed on? 

Set Node Affinity to the deployment to place the PODs on node01 only

* Name: blue
* Replicas: 6
* Image: nginx
* NodeAffinity: requiredDuringSchedulingIgnoredDuringExecution
* Key: color
* values: blue

```
  spec:
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: color
                operator: In
                values:
                - blue
```
### Create a new deployment named 'red' with the NGINX image and 3 replicas, and ensure it gets placed on the master node only.

Use the label - node-role.kubernetes.io/master - set on the master node.

 affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: node-role.kubernetes.io/master
                operator: Exists
Apply a label color=blue to node node01
```
master $ kubectl get node node01  --show-labels
NAME      STATUS    ROLES     AGE       VERSION   LABELS
node01    Ready     <none>    48m       v1.11.3   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/hostname=node01
master $ kubectl label node01 color=blue

master $ kubectl label node node01 color=blue
node/node01 labeled
```
### Create a new deployment named 'blue' with the NGINX image and 6 replicas
```
kubectl run blue --image=nginx --replicas=6
```
### Deploy a pod named nginx-pod using the nginx:alpine image.
```
kubectl run --generator=run-pod/v1 nginx-pod --image=nginx:alpine
kubectl run --generator=run-pod/v1 redis --image=redis:alpine --labels=tier=db
```
### Create a deployment named webapp using the image kodekloud/webapp-color with 3 replicas
```
master $ kubectl create deployment webapp --image=kodekloud/webapp-color
master $ kubectl scale deployment.apps/webapp --replicas=3
```
### Expose the webapp as service webapp-service application on port 30082 on the nodes on the cluster. 
```
kubectl expose deployment webapp --type=NodePort --port=8080 --name=webapp-service --dry-run -o yaml > webapp-service.yaml
```
