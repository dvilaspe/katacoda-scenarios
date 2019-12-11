## Step 03
```
cat > etcd-operator-deployment.yaml<<EOF
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    name: etcdoperator
  name: etcd-operator
spec:
  replicas: 1
  selector:
    matchLabels:
      name: etcd-operator
  template:
    metadata:
      labels:
        name: etcd-operator
    spec:
      containers:
      - command:
        - etcd-operator
        - --create-crd=false
        env:
        - name: MY_POD_NAMESPACE
          valueFrom:
            fieldRef:
              apiVersion: v1
              fieldPath: metadata.namespace
        - name: MY_POD_NAME
          valueFrom:
            fieldRef:
              apiVersion: v1
              fieldPath: metadata.name
        image: quay.io/coreos/etcd-operator@sha256:c0301e4686c3ed4206e370b42de5a3bd2229b9fb4906cf85f3f30650424abec2
        imagePullPolicy: IfNotPresent
        name: etcd-operator
      serviceAccountName: etcd-operator-sa
EOF

kubectl -n myproject create -f etcd-operator-deployment.yaml

kubectl -n myproject get deploy

kubectl -n myproject get pods

export ETCD_OPERATOR_POD=$(kubectl -n myproject get pods -l name=etcd-operator -o jsonpath='{.items[0].metadata.name}')
kubectl -n myproject logs $ETCD_OPERATOR_POD -f

kubectl -n myproject get endpoints etcd-operator -o yaml

```