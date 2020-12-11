Here only simply record my learning notes. 


# Minikube
I choose the minikube to be my tool of creating cluster.

Basically we can use this command below, but it will launch the default version of kubernetes.
```
minikube start --driver=docker
```
Hence, I will assign a specific version of k8s.
```
minikube start --kubernetes-version v1.15.12 
or
minikube start --kubernetes-version v1.15.12 --driver=docker
```

---
# kubectl based commands 

- Create a container (create an image) by kubectl
```
kubectl create deployment hello-minikube --image=k8s.gcr.io/echoserver:1.10
```

- Let your image start service
```
kubectl expose deployment hello-minikube --type=NodePort --port=8080
```

- Check the status
```
kubectl get pod
```

- Get the public url
```
minikube service hello-minikube --url
```

## Delete the instance / cluster
- First, delete the service
``` 
kubectl delete services hello-minikube
```

- Second, delete the deployment
```
kubectl delete deployment hello-minikube
```

- Use minikube to stop the k8s cluster
```
minikube stop
```

- Delete the cluster
```
minikube delete
```