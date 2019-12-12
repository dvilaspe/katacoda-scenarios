## Pre-Instalacion
```
sudo minikube stop
sudo minikube delete

docker system prune -a
#check system clear
docker ps -a
docker images
#...

sudo minikube start --vm-driver=none --kubernetes-version=1.14.10


```