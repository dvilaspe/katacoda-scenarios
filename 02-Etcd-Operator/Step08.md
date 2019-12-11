## Step 8
```
kubectl -n myproject delete etcdcluster example-etcd-cluster
kubectl -n myproject delete deployment etcd-operator
kubectl -n myproject delete crd etcdclusters.etcd.database.coreos.com

```