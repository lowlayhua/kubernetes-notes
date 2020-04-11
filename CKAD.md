# Secret
# Create a secret file
```
master $ more secret.yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
data:
  DB_Host: c3FsMDEK
  DB_User: cm9vdAo=
  DB_Password: cGFzc3dvcmQxMjMK
```
 
# Define in the Pod defination file.
```
apiVersion: v1
kind: Pod
metadata:
  name: envfrom-secret
spec:
  containers:
  - name: envars-test-container
    image: nginx
    envFrom:
    - secretRef:
        name: db-secret
```
# RBAC
```
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: read-pods
  namespace: default
subjects:
- kind: ServiceAccount
  name: dashboard-sa # Name is case sensitive
  namespace: default
roleRef:
  kind: Role #this must be Role or ClusterRole
  name: pod-reader # this must match the name of the Role or ClusterRole you wish
 to bind to
  apiGroup: rbac.authorization.k8s.io
 ```
 
 # Taints
 ### Create a taint on node01 with key of 'spray', value of 'mortein' and effect of 'NoSchedule'
```
kubectl taint nodes node01 spray=mortein:NoSchedule
```

### List the nodes in your cluster, along with their labels:
 ```
 kubectl get nodes --show-labels
 ```
 ### Add label
 ```
 kubectl label nodes <your-node-name> disktype=ssd
 ```
 
 ### Set Node Affinity to the deployment to place the PODs on node01 only
 ```
 apiVersion: apps/v1
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
# red
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: red
spec:
  replicas: 3
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
              - key: node-role.kubernetes.io/master
                operator: Exists
```
