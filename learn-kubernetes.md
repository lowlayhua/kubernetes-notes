https://kubernetes.io/docs/tutorials/kubernetes-basics/create-cluster/cluster-interactive/

# Module 1
```
kubectl version
minikube start
kubectl version
kubectl cluster-info
kubectl cluster-info dump
kubectl get nodes
```
#  Using kubectl to Create a Deployment

## Create a deployment
```
kubectl create deployment hello-node --image=gcr.io/hello-minikube-zero-install/hello-node --port=8080
kubectl get deployments
kubectl get events
kubectl config view
```
```
kubectl create -f https://k8s.io/examples/controllers/nginx-deployment.yaml
kubectl rollout status deployment.v1.apps/nginx-deployment
kubectl get rs
```

## Create a service
```
kubectl expose deployment hello-node --type=LoadBalancer
kubectl get services


kubectl run kubernetes-bootcamp --image=gcr.io/google-samples/kubernetes-bootcamp:v1 --port=8080
kubectl proxy
export POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}')
echo Name of the Pod: $POD_NAME

kubectl get pods
kubectl get pods — all-namespaces
kubectl describe pods
kubectl describe nodes
kubectl describe deployments
```

## kubectl proxy
```
export POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}')
echo Name of the Pod: $POD_NAME
curl http://localhost:8001/api/v1/namespaces/default/pods/$POD_NAME/proxy/
kubectl exec $POD_NAME env
kubectl exec -ti $POD_NAME bash
```
# create a new service
```
kubectl get pods
kubectl get services
kubectl expose deployment/kubernetes-bootcamp --type="nodePort" --port 8080
kubectl get services
kubectl describe services/kubernetes-bootcamp
Name:                     kubernetes-bootcamp
Namespace:                default
Labels:                   run=kubernetes-bootcamp
Annotations:              <none>
Selector:                 run=kubernetes-bootcamp
Type:                     NodePort
IP:                       10.110.105.140
Port:                     <unset>  8080/TCP
TargetPort:               8080/TCP
NodePort:                 <unset>  31423/TCP
Endpoints:                172.18.0.4:8080
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>

export NODE_PORT=$(kubectl get services/kubernetes-bootcamp -o go-template='{{(index .spec.ports 0).nodePort}}')
echo NODE_PORT=$NODE_PORT
```
## scaling apps
```
kubectl get deployments
kubectl scale deployments/kubernetes-bootcamp --replicas=4
kubectl get deployments
kubectl get pods -o wide
kubectl describe deployments/kubernetes-bootcamp

kubectl scale deployments/kubernetes-bootcamp --replicas=2
```
## Load balancing
```
kubectl describe deployments/kubernetes-bootcamp
export NODE_PORT=$(kubectl get services/kubernetes-bootcamp -o go-template='{{(index .spec.ports 0).nodePort}}')
echo NODE_PORT=$NODE_PORT
curl $(minikube ip):$NODE_PORT
```

## Performing a Rolling Update

### Step 1: Performing an update
```
kubectl get deployments
kubectl get pods
kubectl describe pods
kubectl set image deployments/kubernetes-bootcamp kubernetes-bootcamp=jocatalin/kubernetes-bootcamp:2
```
### Step 2: Verify an update
```
kubectl describe services/kubernetes-bootcamp
export NODE_PORT=$(kubectl get services/kubernetes-bootcamp -o go-template='{{(index .spec.ports 0).nodePort}}')
echo NODE_PORT=$NODE_PORT
curl $(minikube ip):$NODE_PORT
kubectl rollout status deployments/kubernetes-bootcamp
kubectl describe pods
```
### Step 3: Rollback an update
```
kubectl set image deployments/kubernets-bootcamp kubernetes-bootcamp=gcr.io/google-samples/kubernetes-bootcamp:v10
kubectl get deployments
kubectl get pods
kubectl describe pods
```
### There is no image called v10 in the repository. Let’s roll back to our previously working version. We’ll use the rollout undo command:
```
kubectl rollout undo deployments/kubernetes-bootcamp
kubectl get pods
kubectl describe pods
```

### Suppose that you made a typo while updating the Deployment, by putting the image name as nginx:1.91 instead of nginx:1.9.1:
```
$ kubectl set image deployment.v1.apps/nginx-deployment nginx=nginx:1.91 --record=true
deployment.apps/nginx-deployment image updated
The rollout will be stuck.

$ kubectl rollout status deployment.v1.apps/nginx-deployment
Waiting for rollout to finish: 1 out of 3 new replicas have been updated...


kubectl rollout history deployment.v1.apps/nginx-deployment
```



## install minikube to run a local kubernetes instance
```
minikube version
minikube start


Checking what nodes you have in your cluster: kubectl get nodes
:minikube start --cpus=4 --memory=4000 --kubernetes-version=v1.7.2

if for any reason your Minikube becomes unstable, or you want to start afresh, minikube stop & minikube delete. Then a minikube start will give you a fresh installation.

start ghost microblogging platform on minikube: kubectl run ghost --image=ghost:0.9 && kubectl expose deployments ghost --port=2368 --type=NodePort
monitor the pod manually: kubectl get pods && minikube service ghost

the kubectl run command is called a generator

minikube dashboard

kuberctl get pods,rs,deployments
kuberctl logs xxxxx
```

# Kubernetes cluster
```
systemctl status kubelet
ls -l /etc/kubernetes/manifests
cat /etc/kubernetes/manifest/etcd.yml

Create a kubernetes cluster on Google kubernetes engine. By default, create 3 3 worker nodes. The master node is being managed by GKE service and cannot be accessed :gcloud container clusters create oreilly

To use GKE, you will first need to do a few things:
- create an account on Google cloud Platform with billing enabled.
- create a project & enable GKE service in it.
- install gcloud CLI on your machine.

To speed up the setup of gcloud, you can make use of "Google Cloud Shell", a pure online browser-based solution.
:gcloud  container clusters list

The gcloud CLI allows you to resize your cluster, update it and upgrade it.
Once you are done using the cluster, do not forget to delete it to avoid being charged.: gcloud container clusters delete  oreilly
```
## Kubernetes Client
```
TO list all pods: kubectl get pods
To list all services, deployments: kubectl get servies,deployments
To list a specific deployment: kubectl get deployment myfirstk8app
To list all resources: kubectl get all
```
## Deleting resources
```
Delete all resources in the namespace: kubectl get ns && kubectl delete ns my-app
Delete service, deployments labelled with app=niceone: kubectl delete svc,deploy -l app=niceone
Force deletion of a pod: kubectl delete pod hangingpod --grace-period=0 --force
Delete all pods in the namespace test: kubectl delete pods --all --namespace test
```
## Watching resoures changes with kubectl
```
$ kuberctl get pods --watch
$ watch kubectl get pods
```

## Editing Resources with kubect
```
$ kubectl run nginx --image=nginx
$ kubectl edit deploy/nginx
```
## Asking kubectl to explain resoures & fields
```
$ kubectl explain svc
$ kubectl explain svc.spec.externalIPs
```
