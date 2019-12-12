## Step 2
```

kubectl create namespace myproject

cat > etcd-alpha-subscription.yaml <<EOF
apiVersion: operators.coreos.com/v1alpha1
kind: Subscription
metadata:
  name: etcd
  namespace: myproject 
spec:
  channel: singlenamespace-alpha
  installPlanApproval: Manual
  name: etcd
  source: operatorhubio-catalog
  sourceNamespace: olm
  startingCSV: etcdoperator.v0.9.4
EOF

kubectl -n myproject create -f etcd-alpha-subscription.yaml

kubectl -n myproject get subscription
kubectl -n myproject get installplan

```