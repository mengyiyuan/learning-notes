## Probes

Allow you to customize how Kubernetes determines the status of your containers.

- Liveness probe: indicates whether the container is running properly, and governs when the cluster will automatically stop or restart the container
- Readiness probe: indicates whether the container is ready to service requests, and governs whether requests will be forwarded to the pod.

Liveness/readiness probes can determine container status by doing things like running a command or making an http request.

## Monitoring apps
With a working metrics server, you can use `kubectl top` to gather information about resource usage within the cluster.
- `kubectl top pods` to get resource uasge for all pods in the default namespace
- `kubectl top nodes` to get resource usage info for nodes

### Debugging
- use `--all-namespaces` as a shortcut to list objects from all namespaces
- to get a clean yaml definition that you can then edit, use `kubectl get <object_type> <object_name> -o yaml --export`
