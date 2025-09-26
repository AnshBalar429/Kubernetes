## Kubernetes Services

There are three types of services:
1. NodePort
2. ClusterIp
3. LoadBalancer


### Commands :
```shell
k get service
```

```shell
k get svc
```

---

### To forward the port :
```shell
k port-forward svc/nginx-service-nodeport 8080:80
```

---

### 🔑 Ways to access a Service inside Kubernetes

#### For same namespace
```shell
http://nginx-service-nodeport
```

#### For different namespace 
```shell
http://nginx-service-nodeport.default.svc.cluster.local
```


#### By ClusterIp
```shell
http://<ClusterIP>:80
```

---