# LAB
## Certificate API


* Identify the certificate file used for the kube-api server
```
/etc/kubernetes/pki/apiserver.crt
```
* Identify the Certificate file used to authenticate kube-apiserver as a client to ETCD Server
```
/etc/kubernetes/pki/apiserver-etcd-client.crt
```
* Identify the key used to authenticate kubeapi-server to the kubelet server
```
/etc/kubernetes/pki/apiserver-kubelet-client.key
```
* Identify the ETCD Server Certificate used to host ETCD server
```
/etc/kubernetes/pki/etc/server.crt
```
* Identify the ETCD Server CA Root Certificate used to serve ETCD Server
```
check /etc/kubernetes/manifests/etcd.yaml
/etc/kubernetes/pki/etcd/ca.crt
```
What is the Common Name (CN) configured on the Kube API Server Certificate?
OpenSSL Syntax: openssl x509 -in file-path.crt -text -noout
```
openssl x509 -in apiserver.crt -text
```
