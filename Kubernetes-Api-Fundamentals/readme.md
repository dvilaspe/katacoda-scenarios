## Step 1
```bash
kubectl create namespace myproject
cat > pod-multi-container.yaml <<EOF
apiVersion: v1
kind: Pod
metadata:
  name: my-two-container-pod
  namespace: myproject
  labels:
    environment: dev
spec:
  containers:
    - name: server
      image: nginx:1.13-alpine
      ports:
        - containerPort: 80
          protocol: TCP
    - name: side-car
      image: alpine:latest
      command: ["/usr/bin/tail", "-f", "/dev/null"]
  restartPolicy: Never
EOF
kubectl -n myproject create -f pod-multi-container.yaml
kubectl -n myproject describe pod my-two-container-pod
kubectl -n myproject exec -it my-two-container-pod -c server -- /bin/sh

ip address
netstat -ntlp
hostname
ps
exit

kubectl -n myproject exec -it my-two-container-pod -c side-car -- /bin/sh

ip address
netstat -ntlp
hostname
ps
exit
```

## Step 2
```
kubectl api-versions
kubectl -n myproject get pods --v=8
kubectl proxy --port=8001 #en otra consola
curl -X GET http://localhost:8001
curl localhost:8001/openapi/v2
curl -X GET http://localhost:8001/api/v1/pods
curl -X GET http://localhost:8001/api/v1/pods | jq .items[].metadata.name
curl -X GET http://localhost:8001/api/v1/namespaces/myproject/pods
curl -X GET http://localhost:8001/api/v1/namespaces/myproject/pods/my-two-container-pod
kubectl -n myproject get pods my-two-container-pod --export -o json > podmanifest.json #deprecated
sed -i 's|nginx:1.13-alpine|nginx:1.14-alpine|g' podmanifest.json
curl -X PUT http://localhost:8001/api/v1/namespaces/myproject/pods/my-two-container-pod -H "Content-type: application/json" -d @podmanifest.json
curl -X PATCH http://localhost:8001/api/v1/namespaces/myproject/pods/my-two-container-pod -H "Content-type: application/strategic-merge-patch+json" -d '{"spec":{"containers":[{"name": "server","image":"nginx:1.15-alpine"}]}}'
curl -X DELETE http://localhost:8001/api/v1/namespaces/myproject/pods/my-two-container-pod
kubectl -n myproject get pods
curl -X GET http://localhost:8001/api/v1/namespaces/myproject/pods/my-two-container-pod
```
## Step 3
```
kubectl -n myproject get pods
cat > replica-set.yaml <<EOF
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myfirstreplicaset
  namespace: myproject
spec:
  selector:
    matchLabels:
     app: myfirstapp
  replicas: 3
  template:
    metadata:
      labels:
        app: myfirstapp
    spec:
      containers:
        - name: nodejs
          image: openshiftkatacoda/blog-django-py
EOF
kubectl -n myproject apply -f replica-set.yaml
kubectl -n myproject get pods -l app=myfirstapp --show-labels -w # En nuevo Terminal
kubectl -n myproject delete pod -l app=myfirstapp
kubectl -n myproject scale replicaset myfirstreplicaset --replicas=6
kubectl -n myproject scale replicaset myfirstreplicaset --replicas=3
curl -X GET http://localhost:8001/apis/apps/v1/namespaces/myproject/replicasets/myfirstreplicaset/scale
curl  -X PATCH localhost:8001/apis/apps/v1/namespaces/myproject/replicasets/myfirstreplicaset/scale -H "Content-type:  application/merge-patch+json" -d ' {"spec":{"replicas":5}}'
curl -X GET http://localhost:8001/apis/apps/v1/namespaces/myproject/replicasets/myfirstreplicaset/status
```

## Step 4
```
cat > finalizer-test.yaml<<EOF
apiVersion: apps/v1
kind: Deployment
metadata:
  name: finalizer-test
  namespace: myproject
  labels:
    app: finalizer-test
  finalizers:
    - finalizer.extensions/v1beta1  
spec:
  replicas: 3
  template:
    metadata:
      labels:
        app: finalizer-test
    spec:
      containers:
        - name: hieveryone
          image: openshiftkatacoda/blog-django-py
          imagePullPolicy: Always
          ports:
            - name: helloworldport
              containerPort: 8080
  selector:
    matchLabels:
      app: finalizer-test
EOF
kubectl -n myproject create -f finalizer-test.yaml
kubectl -n myproject get deploy
kubectl -n myproject get replicaset
kubectl -n myproject get pods
kubectl -n myproject delete deployment finalizer-test
kubectl -n myproject get deployment finalizer-test -o yaml | grep 'deletionGracePeriodSeconds\|deletionTimestamp'
kubectl -n myproject scale deploy finalizer-test --replicas=5
kubectl -n myproject scale deploy finalizer-test --replicas=1
kubectl -n myproject get deploy
kubectl -n myproject get pods
cat > finalizer-test-remove.yaml<<EOF
apiVersion: apps/v1
kind: Deployment
metadata:
  name: finalizer-test
  namespace: myproject
  labels:
    app: finalizer-test
  finalizers:
spec:
  replicas: 3
  template:
    metadata:
      labels:
        app: finalizer-test
    spec:
      containers:
        - name: hieveryone
          image: openshiftkatacoda/blog-django-py
          imagePullPolicy: Always
          ports:
            - name: helloworldport
              containerPort: 8080
  selector:
    matchLabels:
      app: finalizer-test
EOF
kubectl -n myproject replace -f finalizer-test-remove.yaml
kubectl -n myproject get deploy
kubectl -n myproject get pods
```

## Step 5
```
cat >> postgres-crd.yaml <<EOF
apiVersion: apiextensions.k8s.io/v1beta1
kind: CustomResourceDefinition
metadata:
  name: postgreses.rd.example.com
spec:
  group: rd.example.com
  names:
    kind: Postgres
    listKind: PostgresList
    plural: postgreses
    singular: postgres
    shortNames:
    - pg
  scope: Namespaced
  version: v1alpha1
EOF
kubectl -n myproject create -f postgres-crd.yaml
curl http://localhost:8001/apis | jq .groups[].name
kubectl -n myproject api-versions
curl http://localhost:8001/apis/rd.example.com/v1alpha1 | jq
kubectl -n myproject get postgres
cat >> wordpress-database.yaml <<EOF
apiVersion: "rd.example.com/v1alpha1"
kind: Postgres
metadata:
  name: wordpressdb
spec:
  user: postgres
  password: postgres
  database: primarydb
  nodes: 3
EOF
kubectl -n myproject create -f wordpress-database.yaml
kubectl -n myproject get postgres
kubectl -n myproject get postgres wordpressdb -o yaml
```
