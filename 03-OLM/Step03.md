## Step 3
```
kubectl create namespace myproject

cat > etcd-alpha-subscription.yaml <<EOF
apiVersion: operators.coreos.com/v1alpha1
kind: Subscription
metadata:
  name: etcd
  namespace: myproject 
spec:
  channel: alpha
  name: etcd
  source: rh-operators
  installPlanApproval: Manual
EOF

kubectl -n myproject create -f etcd-alpha-subscription.yaml

kubectl -n myproject get subscription
kubectl -n myproject get installplan

```