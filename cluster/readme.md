To create a cluster 

```
kind create cluster --config .\clusters.yml --name local
```

To delete the cluster

```
kind delete cluster -n local
```

Credentials are stored in 

```cat ~/.kube/config```

this file from where kubectl will pick and authorise to talk to cluster


```
kubectl get pods
```

Watch the command by ```-w``` flag