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
```

Create another pod named 'bee' with the NGINX image, which has a toleration set to the taint Mortein

* Image name: nginx
* Key: spray
* Value: mortein
* Effect: NoSchedule
* Status: Running
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
Apply a label color=blue to node node01
``'kubectl label node node01 color=blue```

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

Create a new deployment named 'red' with the NGINX image and 3 replicas, and ensure it gets placed on the master node only.
Use the label - node-role.kubernetes.io/master - set on the master node.

- Name: red
- Replicas: 3
- Image: nginx
- NodeAffinity: requiredDuringSchedulingIgnoredDuringExecution
- Key: node-role.kubernetes.io/master


```
 affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: node-role.kubernetes.io/master
                operator: Exists
```
## Resource Limits

## Daemonsets
What is the image used by the POD deployed by the weave-net DaemonSet?
```
kubectl describe daemonset weave-net --namespace=kube-system
```

# Static Pods - don't know
Create a static pod named static-busybox that uses the busybox image and the command sleep 1000
```
kubectl run --restart=Never --image=busybox static-busybox --dry-run -o yaml --command -- sleep 1000 > /etc/kubernetes/manifests/static-busybox.yaml
```
We just created a new static pod named static-greenbox. Find it and delete it.
```
Identify which node the static pod is created on, ssh to the node and delete the pod definition file. If you don't know theIP of the node, run the kubectl get nodes -o wide command and identify the IP. Then SSH to the node using that IP. For static pod manifest path look at the file /var/lib/kubelet/config.yaml on node01
```
## Multiple scheduler

Deploy an additional scheduler to the cluster following the given specification.
Use the manifest file used by kubeadm tool. Use a different port than the one used by the current one.

* Namespace: kube-system
* Name: my-scheduler
* Status: Running
* Custom Scheduler Name
```
apiVersion: v1
kind: Pod
metadata:
  annotations:
    scheduler.alpha.kubernetes.io/critical-pod: ""
  creationTimestamp: null
  labels:
    component: my-scheduler
    tier: control-plane
  name: my-scheduler
  namespace: kube-system
spec:
  containers:
  - command:
    - kube-scheduler
    - --address=127.0.0.1
    - --kubeconfig=/etc/kubernetes/scheduler.conf
    - --leader-elect=false
    - --port=10282
    - --scheduler-name=my-scheduler
    - --secure-port 0
    image: k8s.gcr.io/kube-scheduler-amd64:v1.16.0
    imagePullPolicy: IfNotPresent
    livenessProbe:
      failureThreshold: 8
      httpGet:
        host: 127.0.0.1
        path: /healthz
        port: 10282
        scheme: HTTP
      initialDelaySeconds: 15
      timeoutSeconds: 15
    name: kube-scheduler
    resources:
      requests:
        cpu: 100m
    volumeMounts:
    - mountPath: /etc/kubernetes/scheduler.conf
      name: kubeconfig
      readOnly: true
  hostNetwork: true
  priorityClassName: system-cluster-critical
  volumes:
  - hostPath:
      path: /etc/kubernetes/scheduler.conf
      type: FileOrCreate
    name: kubeconfig
status: {}
```
# Don't know
A POD definition file is given. Use it to create a POD with the new custom scheduler.
File is located at /root/nginx-pod.yaml
