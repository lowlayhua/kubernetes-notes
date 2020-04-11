# Secret
# Create a secret file
```
master $ more secret.yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
data:
  DB_Host: c3FsMDEK
  DB_User: cm9vdAo=
  DB_Password: cGFzc3dvcmQxMjMK
```
 
# Define in the Pod defination file.
```
envFrom:
    - secretRef:
        name: db-secret
```
