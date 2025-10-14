**Taints** and **Tolerations** are a Kubernetes mechanism that allows you to control which Pods can be scheduled on which Nodes. Taints are applied to Nodes, and Tolerations are applied to Pods.


## Taints (Applied to Nodes)

A **Taint** is a label you place on a Node to mark it as "tainted." This tells the Kubernetes scheduler to avoid placing Pods on this Node unless the Pod explicitly tolerates the taint.

You can apply a taint to a node using `kubectl`. The taint consists of a key, a value, and an effect.

`kubectl taint nodes <node-name> <key>=<value>:<effect>`

### Taint Effects

There are three possible effects a taint can have:

1.  **`NoSchedule`**: This is the most common effect. The scheduler will **not** place any new Pods on the tainted node unless they have a matching toleration. Pods that are already running on the node are not affected.
2.  **`PreferNoSchedule`**: This is a "soft" version of `NoSchedule`. The scheduler will **try to avoid** placing Pods on the tainted node, but if there are no other nodes available, it might schedule them there anyway.
3.  **`NoExecute`**: This is the strongest effect. It will not schedule new Pods, and it will also **evict** any existing Pods from the node that do not tolerate the taint. This is often used to remove Pods from a failing or misbehaving node.

#### Example of Tainting a Node:

Let's say you have a node named `gpu-node-1` that you want to reserve for GPU-intensive workloads. You would taint it like this:

`kubectl taint nodes gpu-node-1 gpu=true:NoSchedule`

Now, no regular Pods will be scheduled on `gpu-node-1`.

-----

## Tolerations (Applied to Pods)

A **Toleration** is a property you add to a Pod's specification (`spec`). It tells the scheduler that the Pod is "allowed" to be scheduled on a Node with a matching taint.

A toleration needs to match the taint's key, value, and effect.

### Example of a Pod with a Toleration

Here is the YAML for a Pod that is designed to run a GPU workload. It has a toleration that perfectly matches the taint we applied to our `gpu-node-1`.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: gpu-workload-pod
spec:
  containers:
  - name: my-cuda-app
    image: nvidia/cuda:11.0-base
  tolerations:
  - key: "gpu"
    operator: "Equal"
    value: "true"
    effect: "NoSchedule"
```

**Breakdown of the Toleration:**

  * **`key: "gpu"`**: This matches the taint key.
  * **`operator: "Equal"`**: This specifies that the `value` must be equal. The other operator is `Exists`, which means the toleration matches any taint with that key, regardless of its value.
  * **`value: "true"`**: This matches the taint value.
  * **`effect: "NoSchedule"`**: This matches the taint effect.

Because this Pod has the correct toleration, the Kubernetes scheduler is now allowed to place it on `gpu-node-1`.

-----

## Common Use Cases

  * **Dedicated Nodes**: Reserving a set of nodes for a specific team or application. You can taint the nodes with `team=A:NoSchedule` and give only that team's Pods the corresponding toleration.
  * **Nodes with Special Hardware**: As in the example above, you can taint nodes with special hardware like GPUs or fast SSDs to ensure only workloads that need that hardware are scheduled there.
  * **Taint-based Evictions**: Automatically evicting pods from a node that has a problem. For instance, Kubernetes automatically adds taints like `node.kubernetes.io/unreachable` with a `NoExecute` effect to nodes that lose network connectivity, causing Pods to be evicted and rescheduled elsewhere.