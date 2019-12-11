## Step 07
```
export EXAMPLE_ETCD_CLUSTER_POD=$(kubectl -n myproject get pods -l app=etcd -o jsonpath='{.items[0].metadata.name}')
kubectl -n myproject delete pod $EXAMPLE_ETCD_CLUSTER_POD
```