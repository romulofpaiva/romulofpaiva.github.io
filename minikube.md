## minikube

[minikube](https://minikube.sigs.k8s.io/docs/start/) is local Kubernetes, focusing on making it easy to learn and develop for Kubernetes.

All you need is Docker (or similarly compatible) container or a Virtual Machine environment, and Kubernetes is a single command away: minikube start

**Cheat Sheet**

#### minikube management
- minikube start (start a cluster)
- minikube update-check (verify/update minikube version)
- minikube stop (stop a cluster)
- minikube delete (delete a cluster)

#### cluster management
- kubectl cluster-info
- kubectl cluster-info dump

#### nodes management
- kubectl get nodes (return cluster information)
- kubectl get services -A

#### namespaces management
- kubectl apply -f <namespace config file> (file with configuration of namespaces)
- kubectl get namespaces or kubectl get ns
- kubectl delete -f <namespace config file> (file with configuration of namespaces)

#### containers management
- kubectl apply -f <deployment config file> (file with configuration for containers deployment)
- kubectl get pods -A (lists the pods from all the namespaces)
- kubectl get pods -n <namespace> (list the pods for an especific namespace) 
- kubectl delete pod <pod-name> -n <namespace>


### Creating Namespaces
Namespaces isolate and organize workloads.

1. Create a namespace.yaml file with two different namespaces.

```yaml
---
apiVersion: v1
kind: Namespace
metadata:
  name: development
---
apiVersion: v1
kind: Namespace
metadata:
  name: production
```

2. Execute "kubectl apply -f namespace.yaml" to create the namespaces.

3. Execute "kubectl get namespaces" to list the namespaces. 

4. Execute "kubectl delete -f namespaces.yaml" to delete the namespaces.


### Deploying an application

1. Create a deployment.yaml file with the following configuration.

```yaml
--- 
apiVersion: apps/v1
kind: Deployment
metadata:
  name: pod-info-deployment
  namespace: development
  labels:
    app: pod-info
spec:
  replicas: 3
  selector:
    matchLabels:
      app: pod-info
  template:
    metadata:
      labels:
        app: pod-info
    spec:
      containers:
      - name: pod-info-container
        image: kimschles/pod-info-app:latest
        ports:
        - containerPort: 3000
        env:
          - name: POD_NAME
            valueFrom:
              fieldRef:
                fieldPath: metadata.name
          - name: POD_NAMESPACE
            valueFrom:
              fieldRef:
                fieldPath: metadata.namespace
          - name: POD_IP
            valueFrom:
              fieldRef:
                fieldPath: status.podIP
```

2. Execute the command "kubectl apply -f deployment.yaml" to deploy the containers.

3. Execute the command "kubectl get deployments -n development" to check the containers status.

4. Execute the command "kubectl get pods -n development" to check the pods that the deployment created.

5. Execute the command "kubectl delete pod <pod-name> -n development" to delete the pod from the development namespace. You'll see that it will be replaced automatically by a new pod.
