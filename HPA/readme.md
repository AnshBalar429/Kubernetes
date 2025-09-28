**Metrics server** - The Metrics Server is a lightweight, in-memory store for metrics.
It collects resource usage metrics (such as CPU and memory) from the kubelets and exposes 
them via the Kubernetes API 



This will start the metrics server:

`kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml`


By this the resource utilisation we can see.
`k top pod`
`k top nodes`

