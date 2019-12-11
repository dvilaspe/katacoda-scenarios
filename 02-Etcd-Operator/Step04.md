## Step 4
```
cat > etcd-operator-cr.yaml<<EOF
apiVersion: etcd.database.coreos.com/v1beta2
kind: EtcdCluster
metadata:
  name: example-etcd-cluster
spec:
  size: 3
  version: 3.1.10
EOF

kubectl -n myproject create -f etcd-operator-cr.yaml

kubectl -n myproject get etcdclusters

kubectl -n myproject get pods -l etcd_cluster=example-etcd-cluster -w

kubectl -n myproject get services -l etcd_cluster=example-etcd-cluster

```