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