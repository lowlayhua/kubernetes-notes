# Troubleshooting

## 1. Check Pod
```
k get po
k describe po web
kubectl logs web -f --previous
```

## 2. Check dependent service & applications

# Control Pane Failure
```
k get nodes
k get pods
k get po -n kube-system
```
### Check controlplan services
```
service kube-apiserver status
service kube-controller-manager status
service kube-scheduler status
service kubelet status
service kube-proxy status
```
### Check service logs
```
k logs kube-apiserver-master -n kube-system
sudo journalctl -u kube-apiserver
```

# Worker Node Failure
```
k get nodes
k describe node worker-1
top
df -h
service kubelet status
sudo journalctl -u kubelet
openssl x509 -in /var/lib/kubelet/worker-1.crt
```

