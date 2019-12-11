## Step6
```
kubectl -n myproject patch etcdcluster example-etcd-cluster --type='json' -p '[{"op": "replace", "path": "/spec/size", "value":5}]'

kubectl -n myproject patch etcdcluster example-etcd-cluster --type='json' -p '[{"op": "replace", "path": "/spec/version", "value":3.2.13}]'
```