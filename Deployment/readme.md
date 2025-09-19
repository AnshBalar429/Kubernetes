> [!NOTE]  
> Deployment ensures that only a certain number of Pods are down while they are being updated. By default, it ensures that at least 75% of the desired number of Pods are up (25% max unavailable).
----

### Commands
```shell
k get deployment
```
```shell
k get deploy
```

```shell
k apply -f ./deployment.yml
```

---
### To see the status of deployment:
```shell
k rollout status deploy nginx-deployment
```

### To restart all the pods in a deployment
```shell
k rollout restart deployment nginx-deployment
```

---

### Scaling a Deployment
```shell
k scale deployment/nginx-deployment --replicas=7
```