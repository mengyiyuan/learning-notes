## Services

Create an abstraction layer which provides network access to a dynamic set of pods.

Most services use a selector to determine which pods will receive traffic through the service. Client can receive uninterrupted access by using the service.

`spec.ports.port` specifies the port the service will listen on, and which one clients will use to access it

`spec.ports.targetPort` specifies the port that traffic will be forwarded to on the pods. You can omit the targetPort if port and targetPort are the same.

To list out service mapping to pods, run:
`kubectl get endpoints <service_name>`

### Service types
- ClusterIP: Service exposed within the cluster using an internal IP address. The service is also accessible using the cluster DNS.
- NodePort: Service exposed externally via a port which listens on each node in the cluster. Commonly used for testing purposes.
- LoadBalancer: Only works if your cluster is set up to work with a cloud provider. Service is exposed through a load balancer created on the cloud platform.
- ExternalName: maps service to an **external** address. Used to allow resources within the cluster to access things outside the cluster through a service. This only sets up a DNS mapping, does not proxy traffic.

