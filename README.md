# Docker 2 tier application 

#Docker stack to run the appliaction on swarm
```
docker stack deploy -c compose.yaml personahub 
```

#Using Helm chart to install the application in k8s
```
kubectl create ns personahubdev
helm repo add personahub https://kajjayamsriram.github.io/Docker-2-tier-app/
helm repo list
helm repo update
helm install personahub personahub -n personahubdev
```
#Using ArgoCd to deploy application in k8s
```
kubectl apply -f personahub-argocd.yaml -n argocd
kubectl delete app personahub -n argocd
#Note this needs argocd installation
```
