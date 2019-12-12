## Step 4
```
cat > etcd-cluster.yaml <<EOF
apiVersion: etcd.database.coreos.com/v1beta2
kind: EtcdCluster
metadata:
  name: example-etcd-cluster
spec:
  size: 3
EOF

kubectl -n myproject create -f etcd-cluster.yaml

kubectl -n myproject get etcdcluster
kubectl -n myproject get pods

```