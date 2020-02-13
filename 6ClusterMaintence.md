# 6. Cluster Maintenance

```
kubectl version
kubeadm upgrade plan
```

We will be upgrading the master node first. Drain the master node of workloads and mark it UnSchedulable
```
kubectl drain master --ignore-daemonsets
```

Upgrade the master components to v1.12.0


Upgrade kubeadm tool, then master components, and finally the kubelet. 
Practice referring to the kubernetes documentation page. Note: While upgrading kubelet, 
if you hit dependency issue while running the apt-get upgrade kubelet command, use the 
apt install kubelet=1.12.0-00 command instead
```
apt install kubeadm=1.12.0-00
kubeadm upgrade apply v1.12.0

apt install kubelet=1.12.0-00 to upgrade the kubelet on the master node
```
