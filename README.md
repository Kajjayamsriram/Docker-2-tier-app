# Docker-2-tier-app
Docker 2 tier application deployed in swam cluster

#Using Helm chart to install the application in k8s
```
kubectl create ns personahubdev
helm repo add personahub https://kajjayamsriram.github.io/Docker-2-tier-app/
helm repo list
helm repo update
helm install personahub personahub -n personahubdev
```
