## Step 1
```
git clone https://github.com/operator-framework/operator-lifecycle-manager

kubectl create -f operator-lifecycle-manager/deploy/ocp/manifests/0.7.2/0000_30_00-namespace.yaml

kubectl get namespaces openshift-operator-lifecycle-manager

kubectl -n openshift-operator-lifecycle-manager create -f operator-lifecycle-manager/deploy/ocp/manifests/0.7.2/0000_30_01-olm-operator.serviceaccount.yaml

kubectl -n openshift-operator-lifecycle-manager -n openshift-operator-lifecycle-manager get serviceaccount olm-operator-serviceaccount

kubectl -n openshift-operator-lifecycle-manager get clusterrole system:controller:operator-lifecycle-manager

kubectl -n openshift-operator-lifecycle-manager get clusterrolebinding olm-operator-binding-openshift-operator-lifecycle-manager

for num in {02..05}; do kubectl -n openshift-operator-lifecycle-manager create -f operator-lifecycle-manager/deploy/ocp/manifests/0.7.2/0000_30_$num*; done

kubectl -n openshift-operator-lifecycle-manager get crds

for num in {06,09}; do kubectl -n openshift-operator-lifecycle-manager create -f operator-lifecycle-manager/deploy/ocp/manifests/0.7.2/0000_30_$num*; done

kubectl -n openshift-operator-lifecycle-manager get catalogsource rh-operators

kubectl -n openshift-operator-lifecycle-manager  get configmap rh-operators


for num in {10..13}; do kubectl -n openshift-operator-lifecycle-manager create -f operator-lifecycle-manager/deploy/ocp/manifests/0.7.2/0000_30_$num*; done

kubectl -n openshift-operator-lifecycle-manager get deployments

```