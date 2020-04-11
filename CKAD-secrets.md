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
