## Step 3
```


export INSTALL_PLAN_NAME=$(kubectl -n myproject get InstallPlan -o jsonpath='{.items[0].metadata.name}')

#set to ture
kubectl -n myproject patch InstallPlan ${INSTALL_PLAN_NAME} --type='json' -p '[{"op": "replace", "path": "/spec/approved", "value":true}]'

kubectl -n myproject get InstallPlan install-etcdoperator.v0.9.2-5mjkg -o yaml


kubectl -n myproject get clusterserviceversion
kubectl -n myproject get crd
kubectl -n myproject get sa
kubectl -n myproject get roles
kubectl -n myproject get rolebindings
kubectl -n myproject get deployments

```