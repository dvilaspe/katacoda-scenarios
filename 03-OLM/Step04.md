## Step 4
```
#set to ture
kubectl -n myproject patch InstallPlan install-etcdoperator.v0.9.2-5mjkg --type='json' -p '[{"op": "replace", "path": "/spec/approved", "value":true}]'

kubectl -n myproject get InstallPlan install-etcdoperator.v0.9.2-5mjkg -o yaml


kubectl -n myproject get clusterserviceversion
kubectl -n myproject get crd
kubectl -n myproject get sa
kubectl -n myproject get roles
kubectl -n myproject get rolebindings
kubectl -n myproject get deployments

```