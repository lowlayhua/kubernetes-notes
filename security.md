# Notes for ~/.kube/config
```
k config view
k config use-context user@cluster
```
# base64
```
cat " xxx"  | base64
cat "xxxx" | base64 --decode
```
# LAB
Inspect the environment and identify the authorization modes configured on the cluster.
```
k describe po kube-apiserver-controlplane -n kube-system | grep auth
      --authorization-mode=Node,RBAC
```
Which account is the kube-proxy role assigned to it?
```
k describe rolebinding kube-proxy -n kube-system
```
