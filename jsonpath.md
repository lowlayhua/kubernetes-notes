# Find nodes name
```
 kubectl get nodes -o=jsonpath='{.items[*].metadata.name}'
 ```
# Custom Column
```
k get nodes -o=custom-columns=<COLUMN NAME>:<JSON PATH>
k get nodes -o=custom-columns=NODE:.metadata.name,CPU:.status.capacity.cpu
k get nodes --sort-by=metadata.name

```
## Use a JSON PATH query to identify the context configured for the aws-user in the my-kube-config context file and store the result in /opt/outputs/aws-context-name.


```
kubectl config view --kubeconfig=my-kube-config -o jsonpath="{.contexts[?(@.context.user=='aws-user')].name}" > /opt/outputs/aws-context-name
```
