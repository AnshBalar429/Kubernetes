1. Runs one pod on one node
2. no replicas field needed
3. DaemonSets are used for cluster-level services that should be present on every machine

> [!NOTE]  
> Node Affinity/Tolerations: You can control which nodes the DaemonSet's Pods run on using nodeSelector, affinity, and tolerations. This allows you to deploy Pods only on nodes with specific labels (e.g., nodes with GPUs) or to run them even on tainted nodes (like the control plane).