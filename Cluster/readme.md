# Cluster Management with Kind

### Create a Cluster
```bash
kind create cluster --config .\clusters.yml --name local
```

### Delete the Cluster
```bash
kind delete cluster -n local
```

### Credentials
Credentials are stored in:
```bash
cat ~/.kube/config
```
This is the file from which `kubectl` will pick and authorize access to the cluster.

### Verify
List pods:
```bash
kubectl get pods
```

Watch the command with the `-w` flag.
