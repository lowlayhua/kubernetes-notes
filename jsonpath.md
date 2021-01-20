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
