Inspect the environment and identify the authorization modes configured on the cluster.
```
k describe po kube-apiserver-controlplane -n kube-system | grep auth
      --authorization-mode=Node,RBAC
```
Which account is the kube-proxy role assigned to it?
```
k describe rolebinding kube-proxy -n kube-system
```
