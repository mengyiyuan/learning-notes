## Deployments

Provide a declarative way to manage a dynamic set of replica pods. They provide functionalities such as scaling and rolling updates.

A deployment defines a **desired state** for the replica pods. The cluster will constantly work to maintain that desired state by creating, removing and modifying the replica pods accordingly.

### Rolling updates and rollbacks
Rolling updates provide a way to update a deployment to a new container version by gradually updating replicas so that there is no downtime.

Execute a rolling update with `kubectl set image deployment/<deployment_name> <container_name>=<image_name> --record

The `--record` flag records information about the update so that it can be rolled back later.

To see the rollout status, run:
`kubectl rollout status deployment/<deployment_name>`

Rollbacks allow us to revert to a previous state.
To get a list of previous updates:
`kubectl rollout history deployment/<deployment name>`

To rollback to a speicif revision:
`kubectl rollout undo deployment/<deployment_name> --to-revision=<revision_number>`

`.spec.strategy.rollingUpdate.maxSurge` is an optional field that specifies the maximum number of Pods that can be created over the desired number of Pods. The value can be an absolute number (for example, 5) or a percentage of desired Pods (for example, 10%).The default value is 25%.

`.spec.strategy.rollingUpdate.maxUnavailable` is an optional field that specifies the maximum number of Pods that can be unavailable during the update process. The value can be an absolute number (for example, 5) or a percentage of desired Pods (for example, 10%). The default value is 25%.