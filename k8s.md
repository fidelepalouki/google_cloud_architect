### Run

####

```bash
kubectl run kubernetes-bootcamp --image=gcr.io/google-samples/kubernetes-bootcamp:v1 --port=8080
```

####

```bash
kubectl get deployments
```

```bash
curl http://localhost:8001/version
```

```bash
export POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}')
echo Name of the Pod: $POD_NAME
```

```bash
curl http://localhost:8001/api/v1/namespaces/default/pods/$POD_NAME/proxy
```

### Exploring the app

```bash
kubectl get pods
kubectl describe pods
```

```bash
export POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}')
echo Name of the Pod: $POD_NAME
```

```bash
curl http://localhost:8001/api/v1/namespaces/default/pods/$POD_NAME/proxy
```

```bash
kubectl logs $POD_NAME
```

```bash
kubectl exec $POD_NAME env
```

#### Next let’s start a bash session in the Pod’s container:

```bash
kubectl exec -ti $POD_NAME bash
```

```bash
cat server.js
```

```bash
curl localhost:8080
```

### Services

```bash
kubectl get pods
```

```bash
kubectl get services
```

```bash
kubectl expose deployment/kubernetes-bootcamp --type="NodePort" --port 8080
```

```bash
kubectl get services
```

```bash
kubectl describe services/kubernetes-bootcamp
```

```bash
export NODE_PORT=$(kubectl get services/kubernetes-bootcamp -o go-template='{{(index .spec.ports 0).nodePort}}')
echo NODE_PORT=$NODE_PORT
```

```bash
curl $(minikube ip):$NODE_PORT
```

#### Using labels

```bash
kubectl describe deployment
```

```bash
kubectl get pods -l run=kubernetes-bootcamp
```

```bash
kubectl get pods -l run=kubernetes-bootcamp
```

```bash
kubectl get services -l run=kubernetes-bootcamp
```

```bash
export POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}')
echo Name of the Pod: $POD_NAME
```

```bash
kubectl label pod $POD_NAME app=v1
```

```bash
kubectl describe pods $POD_NAME
```

```bash
kubectl get pods -l app=v1
```

#### Deleting a service

```bash
kubectl delete service -l run=kubernetes-bootcamp
```

```bash
kubectl get services
```

```bash
curl $(minikube ip):$NODE_PORT
```

```bash
kubectl exec -ti $POD_NAME curl localhost:8080
```

### Scale your app

### Scaling a deployment

```bash
kubectl get deployments
```

```bash
kubectl scale deployments/kubernetes-bootcamp --replicas=4
```

```bash
kubectl get deployments
```

```bash
kubectl get pods -o wide
```

```bash
kubectl describe deployments/kubernetes-bootcamp
```

#### Load balancing

```bash
kubectl describe services/kubernetes-bootcamp
```

```bash
export NODE_PORT=$(kubectl get services/kubernetes-bootcamp -o go-template='{{(index .spec.ports 0).nodePort}}')
echo NODE_PORT=$NODE_PORT
```

```bash
curl $(minikube ip):$NODE_PORT
```

#### Scale down

```bash
kubectl scale deployments/kubernetes-bootcamp --replicas=2
```

```bash
kubectl get deployments
```

```bash
kubectl get pods -o wide
```

### Updating your app

#### Update the version

```bash
kubectl get deployments
```

```bash
kubectl get pods
```

```bash
kubectl describe pods
```

```bash
kubectl set image deployments/kubernetes-bootcamp kubernetes-bootcamp=jocatalin/kubernetes-bootcamp:v2
```

```bash
kubectl get pods
```

#### Verify an update

```bash
kubectl describe services/kubernetes-bootcamp
```

```bash
export NODE_PORT=$(kubectl get services/kubernetes-bootcamp -o go-template='{{(index .spec.ports 0).nodePort}}')
echo NODE_PORT=$NODE_PORT
```

```bash
curl $(minikube ip):$NODE_PORT
```

```bash
kubectl rollout status deployments/kubernetes-bootcamp
```

```bash
kubectl describe pods
```

#### Rollback an update

```bash
kubectl set image deployments/kubernetes-bootcamp kubernetes-bootcamp=gcr.io/google-samples/kubernetes-bootcamp:v10
```

```bash
kubectl get deployments
```

```bash
kubectl get pods
```

```bash
kubectl describe pods
```

```bash
kubectl rollout undo deployments/kubernetes-bootcamp
```

```bash
kubectl get pods
```

```bash
kubectl describe pods
```
