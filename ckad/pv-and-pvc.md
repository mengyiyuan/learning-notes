## PersistentVolume

Just like a Node in a Kubernetes cluster represents CPU/memory resource. PV represents a storage resource.

PersistentVolumeClaim (PVC)
Abstraction layer between user(pod) and the PV

PVCs will automatically **bind** themselves to a PV that has compatible `StorageClass` and `accessModes`.

