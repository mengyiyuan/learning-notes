## API Primitives
Also called Kuberenetes objects.

`kubectl api-resources` will list object types currently available to the cluster.

Every object has a spec and a status:
* spec - provided by you, defines the **desired** state of the object
* status - provided by the cluster, contains information about the actual current state of the object

## Pod
### Container configuration:
* ports: if you need a port that the container is listening on to be exposed to the cluster, you need to specify a `containerPort`

### ConfigMap
A ConfigMap is a Kubernetes object that stores configuration data in a key-value format. This configuration data can be used to configure software running in a container by referencing the ConfigMap in the Pod spec.

1. ConfigMaps can be mapped to environment variables that the containers have access to.
```
env:
- name: MY_VAR
  valueFrom:
    configMapKeyRef:
      name: my-config-map
      key: myKey
```

2. ConfigMap data can be passed to containers as files using a mounted volume:
```
spec:
  containers:
  - name: myapp-container
    image: busybox
    command: ['sh', '-c', "echo $(cat /etc/config/myKey) && sleep 3600"]
    volumeMounts:
      - name: config-volume
        mountPath: /etc/config
  volumes:
    - name: config-volume
      configMap:
        name: my-config-map
```

### SecurityContext ([docs](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/))
A Pod's securityContext defines privilege and access control settings for a pod, defined as a Pod spec. E.g.: `runAsUser` means the Pod will run as the user specified, instead of as `root` if securityContext is not specified.

### Resource requirements ([docs](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/#resource-requests-and-limits-of-pod-and-container))
You can specify the resource requirements of a container in the pod spec with resource requests and limits.
* resource request: the amount of resources neccessary to run a container. A pod will only run on a node that has enough available resources to run the pod's containers.
* resource limit: a max value for the resource usage of a container.

> If a Container exceeds its memory limit, it might be terminated. If it is restartable, the kubelet will restart it, as with any other type of runtime failure.

> If a Container exceeds its memory request, it is likely that its Pod will be evicted whenever the node runs out of memory.

> A Container might or might not be allowed to exceed its CPU limit for extended periods of time. However, it will not be killed for excessive CPU usage.

cpu value in resources is defined as 1/1000 of a CPU core, e.g.: cpu: "250m" means 1/4 of a core.

### ServiceAccounts
Allow containers running in pods to access the kuberenetes API. ServiceAccounts provide a way for applications running in containers to interact with the cluster itself securely with properly limited permissions.

1. Create a serviceaccount
```
kubectl create serviceaccount my-serviceacunt
```
2. Define in Pod spec
```
spec:
  serviceAccountName: my-serviceaccount
```
