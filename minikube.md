## minikube

[minikube](https://minikube.sigs.k8s.io/docs/start/) is local Kubernetes, focusing on making it easy to learn and develop for Kubernetes.

All you need is Docker (or similarly compatible) container or a Virtual Machine environment, and Kubernetes is a single command away: minikube start

**Cheat Sheet**

- minikube start (start a cluster)
- minikube update-check (verify/update minikube version)
- minikube stop (stop a cluster)
- minikube delete (delete a cluster)
- kubectl cluster-info
- kubectl cluster-info dump
- kubectl get nodes (return cluster information)
- kubectl get namespaces
- kubectl get pods -A (lists the pods from all the namespaces)
- kubectl get services -A
