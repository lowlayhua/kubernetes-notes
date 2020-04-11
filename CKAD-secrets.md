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
