# 3. Scheduling
## manual scheduling
```
nodeName: node02
```
## Label and selector
We have deployed a number of PODs. They are labelled with 'tier', 'env' and 'bu'. How many PODs exist in the 'dev' environment?
```
kubectl get pods --selector env=dev
```
Identify the POD which is 'prod', part of 'finance' BU and is a 'frontend' tier?
```
kubectl get all --selector env=prod,bu=finance,tier=frontend
```

Create a taint on node01 with key of 'spray', value of 'mortein' and effect of ‘NoSchedule'
```
kubectl taint nodes node01 spray=mortein:NoSchedule
node/node01 tainted
```

Create another pod named 'bee' with the NGINX image, which has a toleration set to the taint Mortein
info_outline
Hint
Answer file at /var/answers/bee.yaml
Image name: nginx
Key: spray
Value: mortein
Effect: NoSchedule
Status: Running
```
apiVersion: v1
kind: Pod
metadata:
  name: bee
spec:
  containers:
  - image: nginx
    name: bee
  tolerations:
  - key: spray
    value: mortein
    effect: NoSchedule
    operator: Equal
```
Remove the taint on master, which currently has the taint effect of NoSchedule
```
kubectl taint node master node-role.kubernetes.io/master:NoSchedule-
node/master untainted
```
## Node Affinity

Set Node Affinity to the deployment to place the PODs on node01 only

* Name: blue
* Replicas: 6
* Image: nginx
* NodeAffinity: requiredDuringSchedulingIgnoredDuringExecution
* Key: color
* values: blue

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: blue
spec:
  replicas: 6
  selector:
    matchLabels:
      run: nginx
  template:
    metadata:
      labels:
        run: nginx
    spec:
      containers:
      - image: nginx
        imagePullPolicy: Always
        name: nginx
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
