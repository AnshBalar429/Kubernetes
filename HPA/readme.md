**Metrics server** - The Metrics Server is a lightweight, in-memory store for metrics.
It collects resource usage metrics (such as CPU and memory) from the kubelets and exposes 
them via the Kubernetes API 



This will start the metrics server:

```shell
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```


By this the resource utilisation we can see.
```shell
kubectl top pod
```

```shell
kubectl top nodes
```


