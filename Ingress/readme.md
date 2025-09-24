### Ingress in Kubernetes
#### The Two Parts of Ingress
For Ingress to work, you need two components:

> [!NOTE]
> **Ingress Resource:** A Kubernetes object where you define the routing rules. It's just a set of instructions, like a configuration file.

> **Ingress Controller:** An actual application (a Pod running a proxy like Nginx, HAProxy, or Traefik) that runs in your cluster. It reads the Ingress resource's rules and actually implements them by configuring the proxy to route traffic.

You must have an Ingress Controller running in your cluster; creating an Ingress resource by itself does nothing.

If we start multiple apps like frontend pod, backend pod , websocket pod then all these
service will start their own load balancers. So it is hard to rate limiting and certificate management and 
also not a good idea.

Thats why Ingress is used. It routes all the requests that are coming to our load balancer to appropriate 
service. 

Ingress : An api object that manages external access to the services in the cluster, typically HTTP.

Ingress may provide load balancing , SSL termination and name-based virtual hosting.


## nginx-ingress

```shell
k apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.12.1/deploy/static/provider/cloud/deploy.yaml
```

It will also start a ingress-controlled Load Balancer.