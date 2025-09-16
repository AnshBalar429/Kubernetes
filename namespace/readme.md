# Kubernetes Namespaces

A **namespace** in Kubernetes is a way to divide cluster resources between multiple users or teams.  
Every pod is part of a namespace.

---

## View Nodes
```bash
k get nodes
```
This shows all the pods running in the **default namespace**.

---

## Create a Namespace
```bash
k create namespace numerator-namespace
```
You can also add the namespace in the metadata of a `pod.yml` manifest file.

---

## View Pods in a Namespace
```bash
k get pods -n numerator-namespace
```

---

## Set Namespace Context
```bash
k config set-context --current --namespace=numerator-namespace
```

---

## Revert to Default Namespace
```bash
k config set-context --current --namespace=default
```
