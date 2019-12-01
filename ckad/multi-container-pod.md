## Multi-container pod
Multi-container pods provide an opportunity to enhance containers with helper containers that provide additional functionality.

### How can containers interact with one another in a pod?
1. Containers in the same pod share the same network, as if they are running on the same host. They can call each other simply using `localhost` on ports, even when the containers are not exposed to the cluster.
2. Using shared storage volumes: by reading and modifying files in a shared storage volume that is mounted with both containers.
3. Using shared process namespace: containers in the same pod can interact with and signal one another's processes directly. To enable process namespace sharing, set `shareProcessNamespace: true` in pod spec.

### Design patterns
* Sidecar pod: uses a sidecar container that enhances/adds functionality to the main container in some way. E.g.: a sidecar container that syncs files between Github repo and the web container file system.
* Ambassador pod: uses an ambassador container to accept network traffic and pass it onto the main container. E.g.: an ambassador that listens on a custom port and forwards traffic to the main container on its hard-coded port (a legacy app for example).
* Adapter pod: uses an adapter container to change the output of the main container in some way. E.g.: an adapter that formats and decorates log output from the main container.

