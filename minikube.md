## Node server and Dockerfile

### minikube/server.js

```javascript
var http = require("http");

var handleRequest = function(request, response) {
  console.log("Received request for URL: " + request.url);
  response.writeHead(200);
  response.end("Hello World!");
};
var www = http.createServer(handleRequest);
www.listen(8080);
```

### minikube/Dockerfile

```Dockerfile
FROM node:6.14.2
EXPOSE 8080
COPY server.js .
CMD node server.js
```

## Create a minikube cluster

### Start

```bash
minikube start
```

### Dashboard

```bash
minikube dashboard
```

## Create deployment

### Create

```bash
kubectl create deployment hello-node --image=gcr.io/hello-minikube-zero-install/hello-node
```

### View deployments

```bash
kubectl get deployments
```

### View the pod

```bash
kubectl get pods
```

### View cluster events

```bash
kubectl get events
```

### View kubectl configuration

```bash
kubectl config view
```

## Create service

### Expose the pod to the public internet

```bash
kubectl expose deployment hello-node --type=LoadBalancer --port=8080
```

### View the services

```bash
kubectl get services
```

###

```bash
minikube service hello-node
```

### Enable addons

```bash
minikube addons list
```

```bash
minikube addons enable heapster
```

### Get pods and services

```bash
kubectl get pod,svc -n kube-system
```

```bash
kubectl nodes
```

```bash
kubectl cluster-info
```

### Clean up

```bash
kubectl delete service hello-node
kubectl delete deployment hello-node
```

### Optionally, stop the Minikube virtual machine (VM):

```bash
minikube stop
```

### Optionally, delete the Minikube VM:

```bash
minikube delete
```
