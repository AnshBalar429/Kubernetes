
## Why Do Pods Need an Identity?

Most pods don't. Your NGINX web server or your Spring Boot application just needs to run its code and serve network traffic. It has *no business* talking to the Kubernetes API.

However, some pods *do*. The example given, a **CI/CD pod (like Jenkins or Argo CD)**, is perfect. The entire *job* of an Argo CD pod is to talk to the Kubernetes API server to:

* See what `Deployments` currently exist.
* Compare that to a Git repository.
* If they are different, send a request to the API server to `create` or `update` a `Deployment`.

For the API server to accept these requests from the Argo CD pod, the pod must first **authenticate**. It does this by presenting a token associated with its `ServiceAccount`. The API server validates this token and says, "Ah, this request is coming from `argocd-service-account` in the `argo-cd` namespace. Now, I will check if that account is *authorized* (via a RoleBinding) to `create` Deployments."

-----

## How It Works: The Default Behavior

This section explains the *default* (and often insecure) mechanism.

1.  **Creation:** When you create a `namespace` (e.g., `kubectl create ns my-app`), Kubernetes automatically creates a `ServiceAccount` object inside it named `default`.
2.  **Assignment:** When you create a `Pod` and *do not* specify a `serviceAccountName` in its YAML, Kubernetes automatically assigns it the `default` ServiceAccount from its own namespace.
3.  **Token Generation:** This `ServiceAccount` is linked to a **Kubernetes `Secret`**. This `Secret` automatically contains a signed **JWT (JSON Web Token)**. This token is a long, cryptographic string that essentially says, "I am the `default` ServiceAccount in the `my-app` namespace, and I am signed by the cluster's Certificate Authority."
4.  **Automatic Mounting:** This is the key part. By default, a controller in Kubernetes sees that your pod is assigned a ServiceAccount and **automatically mounts** that `Secret`'s data into the pod's filesystem. This appears as a directory at `/var/run/secrets/kubernetes.io/serviceaccount/`.
5.  **Inside the Pod:** If you get a shell inside a default pod (`kubectl exec -it my-pod -- /bin/sh`) and run `ls -l /var/run/secrets/kubernetes.io/serviceaccount/`, you will see files like:
    * `token`: This file contains the actual JWT Bearer token.
    * `ca.crt`: The cluster's public certificate, so the pod can trust it's talking to the real API server (to prevent man-in-the-middle attacks).
    * `namespace`: A convenience file with the pod's namespace.
6.  **Usage:** A program inside the pod (like `kubectl` or a Kubernetes client library) is pre-programmed to look for this `token` file. It reads the token and attaches it to any API request it makes as an HTTP header: `Authorization: Bearer <content-of-the-token-file>`.

-----



So we stop this automatic mounting of Token.