# KUBERNETES PODS | DEPOLY YOUR FIRST APP

### Project Overview

This project demonstrates the basic deployment and management of a Kubernetes Pod on a local Kubernetes cluster using Minikube and kubectl.

The goal is to understand how to create a Pod, verify that it is running, inspect its configuration, and manage it using Kubernetes commands.

### Technologies Used

Kubernetes

Minikube

kubectl

Docker

Windows / WSL2

### Prerequisites

Before starting, ensure the following are installed:

Docker Desktop

Minikube

kubectl


### Install kubectl

First thing is to install kubectl. Search for Kubectl installation
If you're using Windows PowerShell, the easiest way to install kubectl is with winget.

Open PowerShell as Administrator and run:

```
winget install -e --id Kubernetes.kubectl
```

After installation, close and reopen PowerShell, then verify:
```
kubectl version --client
```

### Install local kubernetes cluster
You can use minikube, kind, k3s, microk8s
for this work, we will be using minikube

### Install minikube

Open PowerShell as Administrator and run:

```
winget install Kubernetes.minikube
```
verify installation:

```
minikube version
```

### Make sure Docker Desktop is running

For learning enviroment, we will be using docker rather than introducing VirtualBox or Hyper-V unnecessarily.

check docker:

```
docker --version
```

### Start Minikube
If Docker is working, start Minikube:

```
minikube start --driver=docker
```
Minikube will create a local Kubernetes cluster inside Docker.

verify cluster:
```
minikube status
```
Then:

```
kubectl get nodes
```
Something similar below will show:

```
NAME       STATUS   ROLES           AGE   VERSION
minikube   Ready    control-plane   ...   ...
```
### Installation of pods

You can visit the kubernetes documentation and search for pods, copy the yaml file:

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80
```

Create a yaml file, paste the copied yaml file inside "pod.yaml" inside:

```
vi pods.yml
```

### To create the pods

Run:

```
kubectl create -f pod.yaml
```
The application will be created

### To access pods: 

```
kubectl get pods
```

Something like this will appear:

```
NAME    READY   STATUS              RESTARTS   AGE
nginx   0/1     ContainerCreating   0          68s
```
To get the more details run:

```
kubectl get pods -o wide
```
Output:

```
NAME    READY   STATUS    RESTARTS   AGE    IP           NODE       NOMINATED NODE   READINESS GATES
nginx   1/1     Running   0          101s   10.244.0.5   minikube   <none>           <none>
```

### Log into kubernetes cluster

Run:

```
minikube ssh
```
To see the application running:

```
curl 10.244.0.5
```
### Kubernetes Deployment

Creating a deployment: The following is an example of a Deployment. It creates a ReplicaSet to bring up three nginx Pods:
You can get this following from Kubernetes deployment documentation.

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```
### Create a deployment file and paste the above codes in it, you can edit according to your desired output

```
vim deployment.yml
```
Create the deployment:

```
kubectl apply -f deployment.yml
```
Then:

```
kubectl get deploy
```
To get more details run:

```
kubectl get deploy -o wide
```

### Project Outcome

Successfully deployed and managed an Nginx Kubernetes Pod on a local Minikube cluster using kubectl. This project provided hands-on experience with basic Kubernetes Pod creation, monitoring, troubleshooting, container access, and resource management.
