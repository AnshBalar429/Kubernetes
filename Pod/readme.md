### Port Forwarding :

```bash
k port-forward -n dev-ns nginx 8080:80 
```
---

### Exec into the pod :
```bash
k exec -it -n dev-ns nginx_pod_name -- bash
```