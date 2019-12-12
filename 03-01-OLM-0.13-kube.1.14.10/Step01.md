## Step 1
```
git clone https://github.com/operator-framework/operator-lifecycle-manager

cd operator-lifecycle-manager

kubectl create -f deploy/upstream/quickstart/crds.yaml
kubectl create -f deploy/upstream/quickstart/olm.yaml

#en nueva consola
./scripts/run_console_local.sh

cd ..

```