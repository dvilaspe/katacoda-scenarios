## Step 5
```
cat > etcd-cluster.yaml <<EOF
apiVersion: etcd.database.coreos.com/v1beta2
kind: EtcdCluster
metadata:
  name: example-etcd-cluster
spec:
  size: 3
EOF

kubect -n myproject create -f etcd-cluster.yaml

kubect -n myproject get etcdcluster
kubect -n myproject get pods

```