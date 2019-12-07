## Labels, Selectors and Annotations

### Labels
Key-value pairs attached to Kubernetes objects. They are used for identifying various attributes of objects which can be used to select and group various subsets of those objects.

`kubectl get <object_type> <object_name> --show-labels` to see labels of objects.

`kubectl get pods -l app=my-app` to get all pods with the label value, use `!=` for inequality.

Set-based selectors - `kubectl get pods -l 'environment in (production,development)'`

Chain multiple selectors together - `kubectl get pods -l app=my-app,environment=production`

### Annotations
Similar to labels as in they are used to store custom metadata about objects.
Unlike labels, they cannot be used to select or group objects in Kubernetes.